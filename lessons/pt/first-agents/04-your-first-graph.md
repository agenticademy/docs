# Seu primeiro grafo

Construa seus primeiros agentes · Capítulo 4 de 8 · 20 minutos

Construir um StateGraph com nós e arestas, rotear com uma aresta condicional e executá-lo passo a passo.

## Objetivos

- Ligue START, classify, os dois setores e END em um grafo que compila.
- Execute o grafo de modo que cada ramo seja seguido uma vez.
- Preveja como o grafo roda.

## A leitura

### Nós e arestas

Um StateGraph parte de uma definição de estado, como Annotation.Root({ … }) ou o MessagesAnnotation do capítulo 3. addNode(name, fn) adiciona um nó: uma função que lê o estado e devolve uma atualização, que os reducers do estado mesclam. addEdge(a, b) diz que b roda depois de a. START e END são a entrada e a saída do grafo: uma aresta a partir de START escolhe o primeiro nó, e uma aresta para END termina a execução.

### Ramos

addConditionalEdges(from, router, targets) coloca uma decisão depois de um nó. O router é uma função do estado que devolve o nome do próximo nó, e targets lista os nós que ele pode devolver. É assim que um grafo encaminha um pedido de reembolso para um setor e uma dúvida para outro. Sem um router, duas arestas simples saindo de um nó rodam as duas.

### Compilar, depois executar

compile() verifica a estrutura e devolve um grafo que você pode chamar com invoke() ou stream(). Ele lança um erro se algo estiver errado, por exemplo um nó que nenhuma aresta alcança (UnreachableNodeError). Ele não exige que todo nó chegue a END: um nó sem aresta de saída simplesmente termina a execução ali.

Uma execução avança em supersteps. Em cada um, todo nó que está pronto roda, todos juntos; depois as atualizações deles são mescladas no estado, e as arestas decidem quais nós ficam prontos em seguida. stream() informa a atualização de cada nó à medida que acontece. Se dois nós de um mesmo superstep escrevem a mesma chave, essa chave precisa de um reducer para combiná-las, ou o LangGraph lança um erro.

## Experimente: Ligue um grafo

Desenhe as cinco arestas de START até END; o LangGraph compila o grafo depois de cada uma. Depois execute os dois chamados de exemplo e acompanhe cada superstep.

https://agenticademy.org/pt/paths/first-agents/04-your-first-graph

## Verificação

Como este grafo roda?

Código:

```
new StateGraph(State)
  .addNode("a", a).addNode("b", b).addNode("c", c)
  .addEdge(START, "a")
  .addEdge("a", "b").addEdge("a", "c")
  .addEdge("b", END).addEdge("c", END)
  .compile();
```

- a, depois b, depois c: três supersteps.
- **✓** a, depois b e c juntos em um superstep: dois supersteps.
- a, b e c todos ao mesmo tempo.
- Não compila: um nó não pode ter duas arestas de saída.

**Por quê:** Quando a termina, b e c ficam prontos, então rodam no mesmo superstep e suas atualizações se mesclam.

## O que você já sabe fazer

- Construir e compilar um StateGraph com nós, arestas, START e END.
- Rotear com addConditionalEdges.
- Explicar os supersteps e quando os nós rodam em paralelo.

---

[Voltar para Construa seus primeiros agentes](README.md) · [Próximo: Ferramentas e chamadas de ferramenta](05-tools-and-tool-calls.md)
