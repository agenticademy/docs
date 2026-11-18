# エージェントのループ：最小限のReActエージェント

はじめてのエージェントを作る · 第6章（全8章） · 20分

モデルのノード、ツールのノード、そして条件付きエッジ1本。エージェントのループ全体をLangGraphのグラフで表します。

## 目標

- エージェントが答えるまで、1スーパーステップずつ進めます。
- モデルが何回実行されたかを数えます。
- 実行の流れを順番に並べます。

## 本文

### 考える、動く、観察する、繰り返す

ReAct（reason and act）はエージェントのパターンです。モデルは会話を読み、ツールを求めるか、答えるかのどちらかをします。ツールを求めたら、あなたのコードがツールを実行して結果を会話に加え、モデルがもう一度読みます。モデルがツールを求めずに答えたとき、ループは終わります。

### グラフとしてのループ

LangGraphでは、このループはMessagesAnnotation（メッセージのリストを持つ状態）の上の2つのノードです。agentノードは、ツールをバインドしたモデルをstate.messagesに対して呼び出し、その応答を返します。toolsノードは@langchain/langgraph/prebuiltのToolNodeです。最後のAIMessageにあるすべてのツール呼び出しを実行し、呼び出しごとにToolMessageを1つ追加します。無効な引数は、第5章と同じくエラーのToolMessageになります。

toolsConditionはagentノードのあとのルーターです。最後のメッセージにtool\_callsがあれば"tools"を、なければENDを返します。toolsからagentへ戻るエッジが1本あります。エージェントはこれで全部で、約25行です。

LangGraphはグラフをスーパーステップ単位で実行します。各スーパーステップでは、出番のノードが1回ずつ実行されます。このグラフでは出番のノードが一度に1つだけなので、1スーパーステップが1回のノード実行になり、graph.stream(input, { streamMode: "updates" })はスーパーステップごとに1つの更新を出します。ループが回る様子を見られます。

### 既製のもの、そして上限

@langchain/langgraph/prebuiltのcreateReactAgentは同じグラフを組みます。これは非推奨になり、langchainパッケージのcreateAgentに置き換えられています。一度手で組んでおけば、そうした補助機能が何をしているかがわかります。

モデルがツールを求め続けると、ループは終わらないことがあります。実行のconfigにあるrecursionLimitがスーパーステップの数に上限を設けます（既定は25）。上限に達すると、LangGraphはGraphRecursionErrorを投げます。下で上限を4にしてみてください。この実行には5スーパーステップが必要です。

## 試す：ループを1ステップずつ

3つのステップに1つずつの操作と、任意の4つ目があります。チケットT-48213を送り、エージェントが答えるまでループを進め、モデルの呼び出しを数え、最後にrecursionLimitでループに上限を設けます。

https://agenticademy.org/ja/paths/first-agents/06-the-agent-loop

## 確認問題

チケットT-48213の実行を順番に並べてください。

1. agentノードがタスクをモデルに送る。
2. モデルがget\_ticketを求める。
3. ToolNodeがget\_ticketを実行し、ToolMessageを追加する。
4. モデルがそれを読み、list\_chargesを求める。
5. ToolNodeがlist\_chargesを実行し、ToolMessageを追加する。
6. モデルがツール呼び出しなしで答える。
7. toolsConditionがENDへ振り分ける。

**理由:** モデルの各ターンは、ツールを求めてToolNodeを通るループに入るか、答えて実行を終えるかのどちらかです。モデルは3回実行されました。

## これでできること

- ToolNodeとtoolsConditionで、ReActエージェントをグラフとして組む。
- streamMode "updates"で、実行をメッセージごとに追う。
- どのエージェントのループにも再帰の上限が必要な理由を説明する。

---

[はじめてのエージェントを作るに戻る](README.md) · [次へ：コールバックと人の承認](07-callbacks-and-approval.md)
