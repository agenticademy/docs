# 1回のモデル呼び出し

はじめてのエージェントを作る · 第2章（全8章） · 15分

LangChain.jsでチャットモデルを呼び出し、返ってくるAIMessageを読み、自分のキーを安全に使います。

## 目標

- 台本モデルで呼び出しを実行します。
- 応答の中からトークン数を見つけます。
- コードが何を出力するかを予想します。

## 本文

### 1つのインターフェース、多くのプロバイダー

プロバイダーのパッケージには、ChatAnthropicやChatOpenAIのようなチャットモデルクラスがあります。これらは1つのインターフェースを共有しています。invoke(messages)はメッセージのリストを送り、AIMessageを1つ返します。ただの文字列を渡すこともでき、その場合LangChainはそれをHumanMessageで包みます。stream(messages)は同じ応答を届いた順に断片で返すので、モデルが書いている途中からテキストを表示できます。

オプションはコンストラクターに渡します。model（そのプロバイダーでのモデル名）、temperature（0にすると言い回しが最も安定し、値を上げるほどばらつきます）、maxTokens（支払ってもよい最長の応答）です。ChatOpenAIはconfiguration.baseURLも受け取るので、同じクラスでOpenAI互換のどのサービスとも通信できます。OpenRouter、Hugging Faceのルーター、NVIDIAのAPIカタログなどです。

### 返ってくるもの

AIMessageには、content（普通の応答ならそのテキスト）、tool\_calls（第5章までは空）、usage\_metadata、response\_metadataがあります。usage\_metadataはトークンを数えます。トークンとは、モデルが読み書きするテキストの断片です。input\_tokensは送った分、output\_tokensは応答の分です。プロバイダーはトークン単位で課金し、入力は送るメッセージごとに増えていくので、注目すべきはこの数字です。response\_metadataには、モデル名や停止した理由など、プロバイダーが付け加える情報が入ります。

### キー

APIキーは有料アカウントのパスワードです。コードにもリポジトリにも入れないでください。自分のマシンやサーバーの環境変数から読み込みます。このページでは、接続したキーはこのタブにとどまり、ブラウザからプロバイダーへ直接送られます。学習にはそれで十分ですが、公開するアプリはサーバーからモデルを呼び出すべきです。そうすればキーが訪問者に送られることはありません。無料枠もあり、レート制限があります。

## 試す：リクエストとその応答

呼び出しを実行し、返ってきたAIMessageを開きます。実際の応答を見たい場合は、自分のモデルを接続してください。

https://agenticademy.org/ja/paths/first-agents/02-one-model-call

## 確認問題

モデルは「Hello.」と応答し、使用量を報告するチャットモデルです。これは何を出力しますか？

コード:

```
const reply = await model.invoke("Say hello.");
console.log(typeof reply.content, reply.usage_metadata.output_tokens > 0);
```

- **✓** string true
- object true
- string undefined
- 例外が発生する：invokeにはメッセージの配列が必要。

**理由:** invokeはただの文字列も受け付け、LangChainがそれをHumanMessageで包みます。テキストの応答では、contentは文字列で、usage\_metadataは出力トークンを数えます。

## これでできること

- invoke()とstream()でチャットモデルを呼び出す。
- AIMessageからcontentとトークン使用量を読む。
- baseURLでChatOpenAIをOpenAI互換のサービスに向ける。
- APIキーを置くべき場所と置いてはいけない場所を説明する。

---

[はじめてのエージェントを作るに戻る](README.md) · [次へ：メッセージと状態](03-messages-and-state.md)
