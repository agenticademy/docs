# CommandとSend

高度なエージェント · 第2章（全10章） · 20分

ノードはCommandを使えば、状態の更新と次のノードの選択を1ステップで行えます。またSendを使えば、モデルが求める数だけの並列のワーカーに仕事を振り分けられます。

## 目標

- 状態を更新し、Commandでジャンプするルーターノードを実行します。
- 2つのチケットについて、台本どおりのモデルが選んだ数のチェックにSendで振り分けます。
- 実行を再開します。リデューサーが結果を集め終えていて、1つのノードがそれをまとめます。
- 集めるノードが何を受け取って動くかを予想します。

## 本文

### Command：1回のreturnで更新とルーティング

ノードはふつう状態の更新を返し、次に何を動かすかはグラフのエッジが決めます。代わりに、ノードはnew Command({ update, goto })を返すこともできます。updateは通常のreturnと同じように状態に書き込まれ、gotoは次のノードを指定します。その選択はノードの実行中に行われます。ルーターはチケットを分類し、その分類を保存し、正しいノードへジャンプするところまでを1ステップで行えます。

Commandの行き先はendsで宣言します。addNode("triage", triage, { ends: ["plan", "reply"] })のように書きます。compile()はendsを使って、すべてのノードに到達できるかを確かめます。ここではplanとreplyにはほかの入り口がないので、endsがなければUnreachableNodeErrorを投げます。getGraphAsync()はendsをエッジとして描きます。gotoはノードの静的なエッジに追加されるもので、置き換えはしません。そのため、Commandを返すノードはふつう静的なエッジを持ちません。

### Send：モデルが求める数だけのワーカー

new Send("check", { ticket, check })は、グラフの状態ではなく独自の入力で、ノードの実行を1つ始めます。これをタスクと呼びます。Sendのリストを返す条件付きエッジは、仕事を振り分けます。それらのタスクはすべて次のスーパーステップで同時に動きます。リストはモデルから来ることもあるので、幅は実行時に決まります。あるチケットではチェックが3つ、別のチケットでは5つです。CommandのgotoもSendを受け付けます。

### 結果を集める

タスクはすべて同じキーに書き込むので、前の章と同じく、そのキーにはリデューサーが必要です。スーパーステップの終わりに、リデューサーはそれらの書き込みをタスクの順、つまりSendの順にマージします。タスクが終わった順は関係ありません。ワーカーからのエッジを持つノードは、その次のスーパーステップで1回だけ動き、すべてを読みます。これがグラフの中のmap-reduceです。

### 引き継ぎと、このラボ

サブグラフの中では、new Command({ graph: Command.PARENT, goto })が親グラフのノードへジャンプします。こうして1つのエージェントが別のエージェントに仕事を渡します。次の章ではこれを使ってチームを作ります。

ラボでは、前の章に出てきた架空のショップHaldenのチケット窓口が、1つのグラフを実行します。triageが各チケットをルーティングし、planが台本どおりのモデルにチェックを選ばせ、summariseが回答を書きます。このページはstream()に渡すinterruptAfterとinterruptBeforeで実行を一時停止します。interrupt()にはAsyncLocalStorageが必要ですが、ブラウザー版のLangGraph.jsにはそれがないためです。

## 試す：ルーティング、振り分け、集約

1つのLangGraph.jsグラフで3つのステップを行います。Commandでチケットをルーティングし、モデルが選んだチェックに振り分け、実行を再開して結果を集めてまとめます。

https://agenticademy.org/ja/paths/advanced-agents/02-command-and-send

## 確認問題

台本どおりのモデルが3つのチェックを選び、チェックごとにかかる時間が違います。summariseは何回動き、何を書くでしょうか。

コード:

```
// findings: a ReducedValue whose
// reducer concatenates lists.
// plan returns { checks:
//   ["order", "shipping", "stock"] }
new StateGraph(State)
  .addNode("plan", plan)
  .addNode("check", async ({ check }) =>
    ({ findings: [check] }))
  .addNode("summarise", (s) => ({
    answer: s.findings.join(" + "),
  }))
  .addEdge(START, "plan")
  .addConditionalEdges("plan", (s) =>
    s.checks.map((c) =>
      new Send("check", { check: c })),
    ["check"])
  .addEdge("check", "summarise")
  .addEdge("summarise", END)
  .compile();
```

- 3回。チェックが1つ終わるたびに動き、最初は名前が1つ、次に2つ、最後に3つすべてになる。
- **✓** 3つすべてが終わったあとに1回。Sendの順に"order + shipping + stock"と書く。
- 1回。3つの名前をチェックが終わった順に書くので、実行ごとに変わりうる。
- 1回。"stock"と書く。各チェックの書き込みが前の書き込みを置き換える。

**理由:** 3つのSendは、同じスーパーステップで3つのcheckタスクを始めます。そのスーパーステップが終わると、リデューサーは書き込みをタスクの順、つまりSendの順にマージします。終わった順ではありません。summariseにはcheckからのエッジがあるので、次のスーパーステップで1回だけ動き、3つすべてを読みます。リデューサーがなければ、1つのスーパーステップでfindingsに3回書き込むことは失敗します。

## これでできること

- ノードからCommandを返し、行き先をendsで宣言して、ルーティングと状態の更新を1ステップで行う。
- モデルが実行時に選ぶ幅で、Sendを使って振り分ける。
- 並列の結果をリデューサーで集め、その順番と次のノードが動くタイミングを予想する。

---

[高度なエージェントに戻る](README.md) · [次へ：サブグラフとチーム](03-subgraphs-and-teams.md)
