# Command e Send

Agentes avançados · Capítulo 2 de 10 · 20 minutos

Um nó pode atualizar o estado e escolher o próximo nó em um só passo com Command, e distribuir o trabalho entre workers em paralelo com Send, quantos o modelo pedir.

## Objetivos

- Execute um nó roteador que atualiza o estado e salta com Command.
- Distribua com Send em tantas verificações quantas o modelo roteirizado escolher, para dois tickets.
- Retome a execução: o reducer reuniu os resultados, e um nó os resume.
- Preveja com o que o nó que reúne é executado.

## A leitura

### Command: atualizar e rotear em um só return

Um nó costuma devolver uma atualização do estado, e as arestas do grafo decidem o que roda em seguida. Em vez disso, um nó pode devolver new Command({ update, goto }). update é escrito no estado como um return comum seria, e goto nomeia o próximo nó, escolhido enquanto o nó roda. Um roteador pode classificar um ticket, guardar a classe e saltar para o nó certo em um só passo.

Declare para onde um Command pode ir com ends: addNode("triage", triage, { ends: ["plan", "reply"] }). compile() usa ends para verificar que todo nó pode ser alcançado: aqui plan e reply não têm outra entrada, então sem ends ele lança UnreachableNodeError. getGraphAsync() desenha ends como arestas. Um goto se soma às arestas estáticas do nó; não as substitui, então um nó que devolve Command normalmente não tem nenhuma.

### Send: quantos workers o modelo pedir

new Send("check", { ticket, check }) inicia uma execução de um nó, chamada tarefa, com sua própria entrada em vez do estado do grafo. Uma aresta condicional que devolve uma lista de Sends distribui o trabalho: todas essas tarefas rodam no próximo superstep, ao mesmo tempo. A lista pode vir do modelo, então a largura é decidida em tempo de execução: três verificações para um ticket, cinco para outro. O goto do Command também aceita Sends.

### Reunir os resultados

Todas as tarefas escrevem a mesma chave, então essa chave precisa de um reducer, como no capítulo anterior. No fim do superstep, o reducer mescla as escritas na ordem das tarefas, que é a ordem dos Sends, seja qual for a ordem em que as tarefas terminaram. Um nó com uma aresta vinda dos workers roda então uma vez, no próximo superstep, e lê todas elas. Isso é map-reduce em um grafo.

### Repasses, e este laboratório

Dentro de um subgrafo, new Command({ graph: Command.PARENT, goto }) salta para um nó do grafo pai. É assim que um agente passa trabalho para outro; o próximo capítulo monta equipes com isso.

No laboratório, a central de tickets da Halden, a loja fictícia do capítulo anterior, roda um só grafo: triage roteia cada ticket, plan deixa o modelo roteirizado escolher as verificações, e summarise escreve a resposta. A página pausa as execuções com interruptAfter e interruptBefore, passados para stream(), porque interrupt() precisa de AsyncLocalStorage, que a versão para navegador do LangGraph.js não tem.

## Experimente: Rotear, distribuir, reunir

Três passos em um mesmo grafo do LangGraph.js: roteie um ticket com Command, distribua nas verificações que o modelo escolhe e depois retome a execução para reunir e resumir.

https://agenticademy.org/pt/paths/advanced-agents/02-command-and-send

## Verificação

O modelo roteirizado escolheu três verificações, e cada verificação leva um tempo diferente. Quantas vezes summarise roda, e o que ele escreve?

Código:

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

- Três vezes, uma por verificação terminada: primeiro um nome, depois dois, depois os três.
- **✓** Uma vez, depois das três: "order + shipping + stock", na ordem dos Sends.
- Uma vez, com os três nomes na ordem em que as verificações terminaram, então pode mudar de uma execução para outra.
- Uma vez: "stock". A escrita de cada verificação substitui a anterior.

**Por quê:** Os três Sends iniciam três tarefas check no mesmo superstep. Quando ele termina, o reducer mescla as escritas na ordem das tarefas, que é a ordem dos Sends, não a ordem em que terminaram. summarise tem uma aresta vinda de check, então roda uma vez, no próximo superstep, e lê as três. Sem o reducer, três escritas em findings em um mesmo superstep falhariam.

## O que você já sabe fazer

- Rotear e atualizar o estado em um só passo devolvendo Command de um nó, com seus destinos declarados em ends.
- Distribuir com Send em uma largura que o modelo escolhe em tempo de execução.
- Reunir resultados paralelos com um reducer, e prever a ordem deles e quando o próximo nó roda.

---

[Voltar para Agentes avançados](README.md) · [Próximo: Subgrafos e equipes](03-subgraphs-and-teams.md)
