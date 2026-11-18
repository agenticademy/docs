# 大規模な状態

高度なエージェント · 第1章（全10章） · 20分

LangGraph.js 1.5.1が状態の1つのキーへの書き込みをどうマージするか。リデューサーのあるStateSchemaフィールドとないフィールド、並列の書き込みの順番、OverwriteとUntrackedValue。

## 目標

- リデューサーなしで同じキーに書き込む2つの並列ノードを実行し、エラーを読みます。
- リデューサーを追加し、2つの書き込みが順番にどう組み合わさるかを見ます。
- Overwriteを使って、リデューサーのある値を置き換えます。
- スーパーステップの終わりを予想します。

## 本文

### 1つのスーパーステップ、多くの書き込み

エージェントシステムの第5章で見たように、LangGraphはグラフをスーパーステップ単位で実行します。実行待ちのノードはすべて同じスーパーステップで並列に動き、更新を返します。更新とは、そのノードが変える状態のキーを持つオブジェクトです。すべてが終わると、LangGraphは更新を適用し、状態のチェックポイントを保存します。同じスーパーステップのノードはお互いの更新を見ないので、大事なのは同じキーへの2つの書き込みがどう組み合わさるかです。

### StateSchemaとリデューサー

LangGraph.js 1.1から、状態をStateSchemaで宣言できます。キーごとに1つのフィールドです。Zodのz.string()のような普通のスキーマで指定したフィールドは、最後に書き込まれた値を保持し、1スーパーステップにつき1回の書き込みを受け付けます。同じスーパーステップでそこに2回書き込むと、値が同じでもInvalidUpdateErrorで失敗し、そのスーパーステップのチェックポイントは保存されません。

ReducedValueフィールドにはリデューサーがあります。リデューサーは、LangGraphが書き込みのたびに呼ぶ関数(current, next) => valueで、たとえばリストに追加するのに使います。以前のAPIであるAnnotation.Rootでは、同じものをAnnotation({ reducer, default })で宣言します。

### どの書き込みが先か

リデューサーはスーパーステップの書き込みを1つずつ、タスクパスの順番で受け取ります。ノードが終わった順番ではありません。タスクパスは文字列として比較され、エッジで始まったノードのパスはそのノード名で終わるので、ここではreviewsがspecsより先になります。1.5.0からは、ライブの実行もチェックポインターも、Python版のLangGraphと同じくこの順番を使います。

### OverwriteとUntrackedValue

1.2で追加されたnew Overwrite(value)はリデューサーを飛ばし、キーをvalueにします。たとえば長いリストを要約で置き換えるときに使います。同じスーパーステップでそのキーへのほかの書き込みは捨てられ、1つのスーパーステップで1つのキーに2つのOverwriteを書き込むと失敗します。

UntrackedValueフィールドは、グラフが動いている間だけ値を保持します。ノードはそれを読めますが、どのチェックポイントにも保存されないので、チェックポイントから再開した実行はその値なしで始まります。大きなデータや一時的なデータで、もう一度取得できるものに使います。

### ラボ

架空のアウトドア用品店Haldenが、冬に向けてあるテントを仕入れるべきかを尋ねます。2つのリサーチノードreviewsとspecsが、同じスーパーステップでそれぞれnotesにメモを1つ追加し、そのあとcondenseがメモを要約します。このグラフを3回実行します。notesにリデューサーなし、追加するリデューサーあり、そしてcondenseがOverwriteを返す場合です。

## 試す：スーパーステップのタイムライン

同じグラフを3回実行します。リデューサーなし、リデューサーあり、そしてOverwriteありです。タイムラインには、各スーパーステップが何を書き込み、そのチェックポイントが何を保存したかが表示されます。

https://agenticademy.org/ja/paths/advanced-agents/01-state-at-scale

## 確認問題

reviewsとspecsはどちらもSTARTの次なので、同じスーパーステップで実行されます。そのスーパーステップが終わると何が起きますか？

コード:

```
const State = new StateSchema({
  topic: z.string(),
  notes: new ReducedValue(z.array(z.string()).default(() => []), {
    reducer: (current, next) => current.concat(next),
  }),
});

new StateGraph(State)
  .addNode("reviews", async () => ({ notes: ["reviews"], topic: "Ridge 2" }))
  .addNode("specs", async () => ({ notes: ["specs"], topic: "Ridge 2" }))
  .addEdge(START, "reviews")
  .addEdge(START, "specs");
```

- notesは["reviews", "specs"]、topicは"Ridge 2"になる。
- **✓** topicでInvalidUpdateErrorが出て、実行が失敗する。
- notesでInvalidUpdateErrorが出て、実行が失敗する。
- topicには最後に終わったノードの書き込みが残る。

**理由:** topicは普通のフィールドなので、1スーパーステップにつき1回の書き込みしか受け付けませんが、どちらも"Ridge 2"であっても2回受けました。notesにはリデューサーがあるので、その2つの書き込みはタスクパスの順番でマージされるはずでした。最後に終わった書き込みを残すフィールドはありません。

## これでできること

- StateSchemaで状態を宣言し、キーごとに普通のフィールド、ReducedValue、UntrackedValueのどれかを選ぶ。
- 1つのキーへの並列の書き込みがどう組み合わさり、どの順番になり、いつ失敗するかを予想する。
- Overwriteを使って、リデューサーのある値を置き換える。

---

[高度なエージェントに戻る](README.md) · [次へ：CommandとSend](02-command-and-send.md)
