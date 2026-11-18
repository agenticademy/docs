# Estado em escala

Agentes avançados · Capítulo 1 de 10 · 20 minutos

Como o LangGraph.js 1.5.1 combina as escritas em uma chave do estado: campos de StateSchema com e sem reducer, a ordem das escritas em paralelo, Overwrite e UntrackedValue.

## Objetivos

- Execute dois nós em paralelo que escrevem a mesma chave sem reducer e leia o erro.
- Adicione um reducer e veja como as duas escritas se combinam, em ordem.
- Use Overwrite para substituir um valor com reducer.
- Preveja o fim de um superstep.

## A leitura

### Um superstep, muitas escritas

O LangGraph executa um grafo em supersteps, como em Sistemas de agentes, capítulo 5. Todo nó pendente roda no mesmo superstep, em paralelo, e retorna uma atualização: um objeto com as chaves do estado que ele muda. Quando todos terminam, o LangGraph aplica as atualizações e salva um checkpoint do estado. Os nós de um superstep nunca veem as atualizações uns dos outros, então o que importa é como duas escritas na mesma chave se combinam.

### StateSchema e reducers

Desde o LangGraph.js 1.1 você pode declarar o estado com StateSchema, um campo por chave. Um campo dado como um schema simples, como z.string() do Zod, guarda o último valor escrito e aceita uma escrita por superstep. Duas escritas nele no mesmo superstep falham com InvalidUpdateError, mesmo quando os valores são iguais, e nenhum checkpoint é salvo para esse superstep.

Um campo ReducedValue tem um reducer: uma função (current, next) => value que o LangGraph chama a cada escrita, por exemplo para adicionar a uma lista. A API mais antiga, Annotation.Root, declara a mesma coisa com Annotation({ reducer, default }).

### Qual escrita vem primeiro

Um reducer recebe as escritas de um superstep uma de cada vez, na ordem dos caminhos de tarefa, não na ordem em que os nós terminaram. Os caminhos de tarefa são comparados como texto, e o caminho de um nó iniciado por uma aresta termina com o nome dele, então aqui reviews vem antes de specs. Desde a 1.5.0, as execuções ao vivo e os checkpointers usam essa mesma ordem, como o LangGraph para Python.

### Overwrite e UntrackedValue

new Overwrite(value), adicionado na 1.2, pula o reducer: a chave passa a valer value, por exemplo para substituir uma lista longa por um resumo. As outras escritas nessa chave no mesmo superstep são descartadas, e duas escritas Overwrite em uma chave em um mesmo superstep falham.

Um campo UntrackedValue guarda um valor enquanto o grafo roda: os nós o leem, mas nenhum checkpoint o salva, então uma execução retomada de um checkpoint começa sem ele. Use-o para dados grandes ou temporários que você pode buscar de novo.

### O laboratório

A Halden, uma loja fictícia de artigos para atividades ao ar livre, pergunta se deve estocar uma barraca para o inverno. Dois nós pesquisadores, reviews e specs, adicionam cada um uma nota a notes no mesmo superstep; depois condense resume as notas. Você executa esse grafo três vezes: sem reducer em notes, com um reducer que adiciona e com condense retornando um Overwrite.

## Experimente: Uma linha do tempo de supersteps

Execute o mesmo grafo três vezes: sem reducer, com um reducer e depois com Overwrite. A linha do tempo mostra o que cada superstep escreveu e o que o checkpoint dele salvou.

https://agenticademy.org/pt/paths/advanced-agents/01-state-at-scale

## Verificação

reviews e specs vêm depois de START, então rodam no mesmo superstep. O que acontece quando ele termina?

Código:

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

- notes é ["reviews", "specs"] e topic é "Ridge 2".
- **✓** A execução falha com InvalidUpdateError em topic.
- A execução falha com InvalidUpdateError em notes.
- topic fica com a escrita do nó que terminou por último.

**Por quê:** topic é um campo simples, então aceita uma escrita por superstep, e recebeu duas, mesmo que as duas sejam "Ridge 2". notes tem um reducer, então suas duas escritas seriam combinadas, na ordem dos caminhos de tarefa. Nenhum campo fica com a escrita que terminou por último.

## O que você já sabe fazer

- Declarar o estado com StateSchema, escolhendo para cada chave um campo simples, um ReducedValue ou um UntrackedValue.
- Prever como escritas em paralelo em uma chave se combinam, em que ordem e quando falham.
- Usar Overwrite para substituir um valor com reducer.

---

[Voltar para Agentes avançados](README.md) · [Próximo: Command e Send](02-command-and-send.md)
