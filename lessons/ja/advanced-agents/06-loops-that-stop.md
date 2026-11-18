# 止まるループ

高度なエージェント · 第6章（全10章） · 20分

ループには、モデルが何をしても効く止め方が必要です。再帰の上限、呼び出しの予算、繰り返し呼び出しの検出器、そして「いいえ」と言える人です。

## 目標

- 1つのツール呼び出しを繰り返すエージェントを、GraphRecursionErrorが止めるまで実行します。
- 呼び出しの予算と繰り返し検出器を設定し、どちらが先に作動するかを見ます。
- 危険なツールの前で一時停止し、呼び出しを却下します。
- 確認問題に合格します。

## 本文

### ループには、モデルが選ばない止め方が必要です

エージェントのループは、モデルがツールを求めずに答えたときに終わります。1ターンは、1回のモデル呼び出しと、そこで求められたツールです。たとえば決して変わらない状態を確かめるために、モデルが求め続けると、ターンのたびに料金のかかるモデル呼び出しが増え、新しくわかることは何もありません。そこで、モデルが何をしても効く止め方を加えます。

### LangGraphの上限：recursionLimit

このようなループでは、ノードの1回の実行が1スーパーステップです。実行のconfigにあるrecursionLimitはスーパーステップ数に上限を設け、既定値は25です。それを超えると、LangGraph.jsはlc\_error\_codeがGRAPH\_RECURSION\_LIMITのGraphRecursionErrorを投げます。これは最後の安全網です。数えるのはターンではなくスーパーステップで、止まるのは遅く、チェックポインターがなければ実行の状態はエラーとともに失われます。

### ミドルウェアとしての止め方

ミドルウェアは、LangChain 1.6のcreateAgentがモデル呼び出しの前後で実行するフックです。beforeModelとafterModelのフックは、それぞれグラフの1つのノードになります。modelCallLimitMiddlewareはモデル呼び出しの回数で数える予算です。runLimitは1回の実行を、threadLimitはスレッド全体を数え、exitBehavior 'end'はメッセージを付けて実行を終えます。toolCallLimitMiddlewareは、1つのツールまたはすべてのツールへの呼び出しを数えます。トークンやお金の予算も同じ仕組みです。数え、比べ、止めます。このサイトはこの2つをmodelCallLimitとtoolCallLimitとして再現しており、ラボではmodelCallLimitを使います。

繰り返し検出器は、行き詰まったループを早めに捕まえます。同じツールが同じ引数で、直近の呼び出しの中にthreshold回現れた場合です。ここのrepeatDetectorはこのサイト独自のもので、LangChainには組み込みのものがありません。exitBehavior 'end'なら実行を終え、'continue'なら呼び出しを拒否して、別のことを試すようモデルに伝えます。ここでは各フックがトレースイベントも書くので、どの止め方が作動したかがわかります。

### 止め役としての人

送金のように、「はい」を待つべき呼び出しがあります。interruptBeforeはノードの前でグラフを一時停止させ、チェックポインターが停止中の状態を保ちます。ブレークポイントはそのノードを実行するたびに止まるので、人が必要な呼び出しをコードが選んで表示します。却下するには、updateStateがtoolsノードが実行されたかのようにToolMessageで呼び出しに答えます。入力をnullにしてもう一度呼び出すと実行が再開します。Nodeでは、humanInTheLoopMiddlewareがこれをinterrupt()で行いますが、interrupt()にはAsyncLocalStorageが必要です。ブラウザにはそれがないため、このページはブレークポイントを使います。

## 試す：ループを止める3つの方法

台本どおりに動くエージェントが、保留のままの返金を確かめ続けます。まずrecursionLimitで止め、次に予算と繰り返し検出器で止め、最後に危険なツールの前に人を置いて止めます。

https://agenticademy.org/ja/paths/advanced-agents/06-loops-that-stop

## 確認問題

モデルは毎ターンcheck\_refund({ order: '4471' })を求め、決して答えません。この実行を終えるのはどの止め方ですか。

コード:

```
const agent = createAgent({
  model,
  tools: [checkRefund],
  middleware: [
    modelCallLimit({
      runLimit: 3,
      exitBehavior: 'end',
    }),
    repeatDetector({
      window: 6,
      threshold: 4,
      exitBehavior: 'end',
    }),
  ],
});
// recursionLimit: 25 (the default)
await agent.graph.invoke({
  messages: [task],
});
```

- GraphRecursionError、スーパーステップ25で。
- 繰り返し検出器、4回目の同一呼び出しで。
- **✓** 呼び出しの予算、4回目のモデル呼び出しの前に。
- どれでもない。実行はモデルが答えたときにしか終わらない。

**理由:** 呼び出しの予算はモデル呼び出しを3回まで許し、4回目の前にmodelCallLimitが実行を終えます。そのため、繰り返し検出器が4回目の同一呼び出しを見ることはありません。ここでは1ターンが5スーパーステップなので、実行はスーパーステップ16で終わり、recursionLimitの25よりずっと前です。そして止め方は決してモデルを待ちません。それこそが止め方の役目です。

## これでできること

- recursionLimitを最後の安全網として設定し、GraphRecursionErrorを捕まえる。
- 呼び出しの予算と繰り返し検出器をミドルウェアとして加え、どちらが作動したかをトレースイベントから読む。
- interruptBeforeで危険なツールの前に一時停止し、updateStateで呼び出しを却下する。

---

[高度なエージェントに戻る](README.md) · [次へ：計画を立てるループ](07-planning-loops.md)
