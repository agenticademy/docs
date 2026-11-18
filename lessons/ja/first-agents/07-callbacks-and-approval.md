# コールバックと人の承認

はじめてのエージェントを作る · 第7章（全8章） · 22分

コールバックで実行を見守り、危険なツールの前で一時停止して、人が承認または拒否できるようにします。

## 目標

- コールバックハンドラーを取り付け、そのイベントを見ます。
- 返金ツールの前で一時停止し、承認します。
- 返金を拒否し、エージェントがそれをどう扱うかを見ます。
- 確認問題に合格します。

## 本文

### コールバックは見守る

コールバックハンドラーは、実行が進むにつれてLangChainがそのメソッドを呼び出すオブジェクトです。BaseCallbackHandlerのサブクラスで、handleChatModelStart、handleLLMEnd、handleToolStart、handleToolEndのようなメソッドを持ちます。config.callbacksに渡すと、そのconfigを受け取る入れ子の呼び出しすべてに届きます。ログ、メトリクス、トレースに使います。コールバックは観察するだけです。LangChainはそのメソッドが返す値を無視し、中で投げられたエラーも既定ではログに記録するだけなので、ステップを変えたり止めたりはできません。

### ブレークポイントは一時停止する

ステップの前で実行を止めるには、LangGraphが状態を保存し、あとでそこに戻る必要があります。それがチェックポインターです。compile({ checkpointer: new MemorySaver() })は、config.configurableのthread\_idのもとで、スーパーステップごとにチェックポイントを保存します。interruptBefore: ["approval\_tools"]を加えると、実行はそのノードの前で止まります。

一時停止している間、(await graph.getState(config)).nextが何を待っているかを示し、最後のAIMessageに保留中の呼び出しがあります。承認するには、graph.invoke(null, config)で再開します。入力がnullなら「チェックポイントから続ける」という意味です。拒否するには、先にgraph.updateState(config, { messages: [toolMessage] }, "approval\_tools")を呼びます。あなたのToolMessageが、ノードが実行されたかのようにその呼び出しに答えるので、ノードは飛ばされ、モデルはあなたの拒否を読みます。

ここでのcreate\_refundのように、危険なツールには専用のノードを与えてください。そうすればゲートはそれだけを止めます。

### Nodeで、そしてほかの場所で

Nodeでは、langchainパッケージのcreateAgentがミドルウェアのフック（beforeModel、afterModel、wrapModelCall、wrapToolCall）と、interrupt()で一時停止するhumanInTheLoopMiddlewareを提供します。interrupt()にはAsyncLocalStorageが必要なので、ブラウザではここと同じくブレークポイントを使います。コーディングエージェントも同じ考え方を使います。ツール呼び出しの前に動き、それを拒否できるフックです。

## 試す：返金の前のゲート

操作が1つずつの4つのステップと、任意の5つ目があります。実行のコールバックイベントを見て、返金の前にゲートを置き、1回の実行を承認し、次の実行を拒否します。

https://agenticademy.org/ja/paths/first-agents/07-callbacks-and-approval

## 確認問題

### 質問1/3

interruptBeforeが動くには何が必要ですか？

- **✓** チェックポインターと、configの中のthread\_id。
- コールバックハンドラー。
- 本物のモデル。

**理由:** 一時停止とは、状態を保存してそこに戻ることです。チェックポインターはスレッドのidのもとで状態を保存します。

### 質問2/3

実行がapproval\_toolsの前で一時停止しています。止まったところから続けるにはどうしますか？

- 元の入力でもう一度invokeする。
- **✓** 同じthread\_idでgraph.invoke(null, config)。
- 新しいスレッドを始める。

**理由:** 入力がnullだと、LangGraphは新しい実行を始めずに、保存したチェックポイントから再開します。

### 質問3/3

コールバックハンドラーのhandleToolStartが、create\_refundを見るとfalseを返します。どうなりますか？

- 返金がブロックされる。
- **✓** 何も変わらない。LangChainは戻り値を無視するので、返金は実行される。ステップを止めるには、グラフを一時停止するか、ツールを守る。
- 実行がapproval\_toolsの前で一時停止する。

**理由:** コールバックは見守りと記録のためのものです。制御はグラフの役目です：ここのようにブレークポイントとupdateStateを使うか、ツールのノード内でチェックします。

## これでできること

- コールバックハンドラーで、実行のモデルとツールのイベントを記録する。
- チェックポインターとinterruptBeforeで、ノードの前でグラフを一時停止する。
- getState、updateState、invoke(null)で、保留中のステップを再開または拒否する。

---

[はじめてのエージェントを作るに戻る](README.md) · [次へ：ガードレール](08-guardrails.md)
