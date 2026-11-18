# サブグラフとチーム

高度なエージェント · 第3章（全10章） · 20分

サブグラフからエージェントのチームを組み、状態の共有のしかたを決め、ストリームの中の名前空間を読み、マルチエージェントのパターンをモデル呼び出しとトークンで比べます。

## 目標

- チームのチームを実行し、ストリームの名前空間を読みます。
- チームの中で一時停止し、そのチーム自身の状態を開きます。
- サブエージェントと引き継ぎを、モデル呼び出しとトークンで比べます。
- サブグラフが何をストリームするかを予想します。

## 本文

### サブグラフ

コンパイル済みのグラフは、別のグラフのノードになれます。これがサブグラフです。2つが状態のキーを共有するときは、addNode("research\_team", researchTeam)のように、コンパイル済みのサブグラフそのものがノードになります。サブグラフは親と共有するキーを読み、そこに書いたものは親に戻り、サブグラフだけが持つキーは中にとどまります。

共有するキーがないときは、ラッパーノードからサブグラフを呼び出します。ラッパーノードは親の状態をサブグラフの入力に変換し、その出力を更新に変換して戻します。getGraphAsyncとgetStateが引き続きサブグラフを見つけられるように、addNodeに{ subgraphs: [team] }を渡します。

### 名前空間とストリーミング

サブグラフの実行はそれぞれ、親のスレッドの中にある自分の名前空間にチェックポイントを保存します。名前空間はノードとタスクにちなんで、research\_team:<task id>と名付けられます。stream(input, { streamMode: "updates", subgraphs: true })は[namespace, update]のペアを返し、最上位のグラフは[]です。getGraphAsync({ xray: true })は、research\_team:leadのようなサブグラフ自身のノードも描きます。

実行がサブグラフの中で一時停止している間は、getState(config, { subgraphs: true })がサブグラフ自身の状態も返します。ブラウザではinterrupt()が動きません。AsyncLocalStorageが必要だからです。そのため、このラボはinterruptBeforeで一時停止し、configを各サブグラフに手で渡します。

### チームのチームと引き継ぎ

スーパーバイザーはほかのスーパーバイザーを管理できます。ラボでは、サポート責任者がCommand({ goto })で2つのチームに振り分け、各チームのリーダーが仕事の一部をワーカーに任せます。サブグラフの中のノードは、親のノードに制御を渡すこともできます。new Command({ graph: Command.PARENT, goto: "writer", update })です。これが引き継ぎです。次のエージェントは、あなたがupdateに入れた履歴を持って仕事を引き取ります。

### パターンを選ぶ

LangChainのJSドキュメントは、5つのマルチエージェントのパターンを挙げています。サブエージェント：主エージェントがほかのエージェントをツールとして呼び出し、それぞれの結果を受け取ります。引き継ぎ：担当するエージェントが替わります。スキル：1つのエージェントが、必要なときに専門家のプロンプトを読み込みます。ルーター：1つのステップが入力を分類し、エージェントに送ります。カスタムワークフロー：自分で作るLangGraphのグラフです。エージェントシステムの第5章はtransfer\_to\_<agent>の呼び出しを引き継ぎと呼びましたが、そのスーパーバイザーが使っていたのはサブエージェントでした。結果はどれもスーパーバイザーに戻っていたからです。

ドキュメントはこれらを比べています（2026-11-04に確認）。1回で終わる依頼では、サブエージェントでモデル呼び出しが4回、引き継ぎで3回です。3つの分野にまたがる質問では、サブエージェントで5回と約9Kトークン、引き継ぎで7回以上と約14K以上です。ラボでは、同じ仕事を両方の方法で実行して数えます。

## 試す：チームのチーム

3つのステップ：チームのチームを実行してストリームを読み、チームの中で一時停止してその状態を開き、最後に同じ仕事をサブエージェントと引き継ぎで実行します。

https://agenticademy.org/ja/paths/advanced-agents/03-subgraphs-and-teams

## 確認問題

チームは最上位のグラフとmessagesを共有し、notesはチーム自身のキーです。ループは何を出力しますか。

コード:

```
const Team = Annotation.Root({
  ...MessagesAnnotation.spec,
  notes: Annotation({ reducer: (a, b) => a.concat(b), default: () => [] }),
});
const team = new StateGraph(Team)
  .addNode("work", () => ({ notes: ["n1"], messages: [{ role: "assistant", content: "done" }] }))
  .addEdge(START, "work")
  .compile();
const top = new StateGraph(MessagesAnnotation)
  .addNode("team", team)
  .addEdge(START, "team")
  .compile();

const input = { messages: [{ role: "user", content: "go" }] };
for await (const [ns, update] of await top.stream(input, { streamMode: "updates", subgraphs: true })) {
  const [node, values] = Object.entries(update)[0];
  console.log(ns.length ? "inside" : "top", node, Object.keys(values));
}
```

- **✓** inside work ["notes", "messages"]、次にtop team ["messages"]。まずチーム自身の更新、次に最上位のグラフに戻るものです。
- top team ["notes", "messages"]。ストリームには最上位のグラフしか出ず、サブグラフのキーも一緒に戻ります。
- inside work ["notes", "messages"]、次にtop team ["notes", "messages"]。チームが書いたキーはすべて戻ります。
- エラーを投げます。最上位のグラフにはnotesのキーがないので、チームはそれを書けません。

**理由:** subgraphs: trueにすると、ストリームはサブグラフ自身の更新も、その名前空間["team:<task id>"]の下で返します。これがなければ、topの行しか見えません。チームはノードそのものなので、共有するキーのmessagesが戻ります。notesはチームの中にしかないので、そこにとどまります。そこに書くことは許されています。

## これでできること

- キーを共有するときはサブグラフをノードとして追加し、共有しないときは状態を変換するラッパーノードから呼び出す。
- subgraphs: trueでストリームして各更新の名前空間を読み、一時停止中のサブグラフの状態をgetStateで読む。
- Command.PARENTで親グラフのノードに制御を渡し、スーパーバイザーを束ねるスーパーバイザーを作る。
- サブエージェント、引き継ぎ、スキル、ルーター、カスタムワークフローを、モデル呼び出しとトークンで比べて選ぶ。

---

[高度なエージェントに戻る](README.md) · [次へ：タイムトラベル](04-time-travel.md)
