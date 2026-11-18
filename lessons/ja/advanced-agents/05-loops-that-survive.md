# 止まらないループ

高度なエージェント · 第5章（全10章） · 20分

再試行ポリシー、タイムアウト、エラーハンドラー、チェックポイントがあれば、ツールが失敗してもプロセスが止まっても、長い実行を続けられます。

## 目標

- 不安定なツールを入れて、再試行を見ます。
- 失敗を補償ステップに回します。
- 実行を途中で止め、チェックポイントから再開します。
- 試行の回数と経路を予想します。

## 本文

### 再試行とタイムアウト

ツール呼び出しは一時的に失敗することがあります。混み合ったサービスからの503や、まったく返事がない場合です。LangGraph.js 1.5.1では、ノードにretryPolicyを付けられます。maxAttempts（最初の試行も数えます）、試行の間の待ち時間を決めるinitialIntervalとbackoffFactor、もう一度試す価値のあるエラーを選ぶretryOnです。タイムアウトは、応答しない呼び出しを失敗した試行に変えます。runTimeoutは1回の試行の合計時間に上限を設け、idleTimeoutは進捗（状態の書き込み、コールバックのイベント、heartbeat()の呼び出しなど）のない時間に上限を設けます。どちらもNodeTimeoutErrorを投げ、再試行ポリシーはそれをほかのエラーと同じように扱います。

### エラーハンドラーと補償

試行を使い切ると、LangGraph.js 1.4で追加されたノードのerrorHandlerが、実行を失敗させる代わりに動きます。errorHandlerは、ノードの入力状態と、ノード名とエラーを持つNodeErrorを受け取り、状態の更新かCommand({ goto })を返します。補償に回すのはSagaパターンです。失敗の前に行った作業を取り消すステップで、たとえば予約したのに支払われなかった部屋を解放します。

### 停止と再開

チェックポインターがあると、各スーパーステップの終わりにスレッドへチェックポイントが保存されます。実行のconfigにRunControlを渡してrequestDrain()を呼ぶと、すでに動いているノードは最後まで実行され、チェックポイントが保存され、実行はGraphDrainedを投げます。同じthread\_idで入力をnullにしてもう一度呼び出すと、次のノードから始まります。終わったノードは再実行されません。メモリではなくデータベースに保存するチェックポインターなら、新しいプロセスからでもこれができます。

クラッシュにはこうした予告がありません。動いていたノードは書き込みを失い、再開時にもう一度実行されます。そのため、副作用のある呼び出しにはべき等キーを付けるべきです。サービスが覚えておくキーで、同じリクエストが繰り返されても2回処理せず、最初の結果を返します。

### このラボでは

予約のグラフです。モデルが旅行を計画し、そのあとreserve、charge、confirmがそれぞれ1つのサービスを呼びます。ブラウザにはAsyncLocalStorageがないため、interrupt()と関数型APIのtask()はここでは動きません。ラボはグラフAPIを使い、すべての呼び出しにconfigを手で渡します。2つ目のプロセスは、同じインメモリのMemorySaverの上に作った新しいグラフで、電源を抜く操作は実行のAbortSignalを中断させます。

## 試す：失敗、回復、再開

ツールを不安定にして再試行を見ます。次に決済サービスを止めて補償までの経路を追い、最後にプロセスを止めてスレッドを再開します。

https://agenticademy.org/ja/paths/advanced-agents/05-loops-that-survive

## 確認問題

charge\_cardは毎回の呼び出しで503を投げます。これを実行すると何が起きますか。

コード:

```
const graph = new StateGraph(Trip)
  .addNode("reserve", reserve)
  .addNode("charge", charge, {
    retryPolicy: {
      maxAttempts: 2,
      initialInterval: 100,
    },
    errorHandler: () =>
      new Command({ goto: "release" }),
  })
  .addNode("release", release)
  .addEdge(START, "reserve")
  .addEdge("reserve", "charge")
  .addEdge("charge", END)
  .addEdge("release", END)
  .compile();

await graph.invoke(input);
```

- **✓** charge\_cardは2回呼ばれます。そのあとerrorHandlerが実行をreleaseに送り、invokeは正常に戻ります。
- charge\_cardは最初の試行と2回の再試行で3回呼ばれます。そのあとreleaseが実行されます。
- charge\_cardは2回呼ばれます。そのあとinvokeが503を投げます。
- 最初の503のあとにerrorHandlerが動き、そのあとchargeがもう一度試されます。

**理由:** maxAttemptsは最初の試行も含めてすべての試行を数えるので、charge\_cardが呼ばれるのは3回ではなく2回です。errorHandlerはそれらの試行を使い切ったあとにだけ動き、試行の間には動きません。そのCommandが実行をreleaseに送り、releaseはENDで終わるので、invokeは状態を返します。errorHandlerのないノードでは、試行を使い切るとinvokeが503を投げます。

## これでできること

- ノードにretryPolicyとタイムアウトを付け、何回試行するかを予想できます。
- errorHandlerとCommandで、ノードの最後の失敗を補償ステップに回せます。
- RunControl.requestDrainで実行を止め、終わったノードを繰り返さずにチェックポイントから再開できます。
- べき等キーを使い、クラッシュのあとに再実行されたノードが2回処理しないようにできます。

---

[高度なエージェントに戻る](README.md) · [次へ：止まるループ](06-loops-that-stop.md)
