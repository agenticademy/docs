# Orquestração: supervisores, fan-out, subgrafos

Sistemas de agentes e padrões · Capítulo 5 de 8 · 25 minutos

Padrões para vários agentes em um grafo LangGraph: um supervisor que roteia, Send para trabalho em paralelo, subgrafos, e o checkpoint que cada superstep salva.

## Objetivos

- Veja dois especialistas rodarem no mesmo superstep.
- Abra a execução de um subgrafo aninhado.
- Execute o mesmo trabalho com um agente e com um supervisor.
- Preveja o fan-out.

## A leitura

### Supersteps

O LangGraph executa um grafo em supersteps. Em cada superstep, todo nó pendente roda, em paralelo; depois, as atualizações deles são mescladas no estado pelo reducer de cada chave, e as arestas decidem quais nós rodam em seguida. As mensagens usam um reducer que acrescenta, então dois nós podem adicionar mensagens no mesmo superstep.

### Threads e checkpoints

Compile um grafo com um checkpointer, como o MemorySaver, e passe um thread\_id ao executá-lo. O LangGraph salva um checkpoint do estado inteiro nessa thread: um para a entrada, depois um ao fim de cada superstep. getState({ configurable: { thread\_id } }) lê o mais recente: os valores, o número do passo e next, os nós que ainda vão rodar. Uma execução posterior na mesma thread começa desse estado.

### Supervisor, fan-out, subgrafos

Um supervisor é um agente que decide qual especialista trabalha em seguida. Aqui, um handoff é uma chamada de ferramenta como transfer\_to\_customer\_support, então o modelo a escolhe como qualquer outra ferramenta.

O fan-out executa trabalho em paralelo. Uma aresta condicional pode devolver uma lista de objetos Send em vez de nomes de nós. Cada Send inicia uma execução de um nó com sua própria entrada, todas no superstep seguinte, e o reducer mescla o que elas devolvem.

Um subgrafo é um grafo compilado usado dentro de um nó. Ele tem seu próprio estado e seu próprio namespace de checkpoints, e suas execuções ficam aninhadas sob o nó que o chamou. Neste lab, o Financial Analyst chama um subgrafo Data Engineer próprio. Handoffs com Command e subgrafos que compartilham estado vêm depois, na trilha Agentes avançados.

### Quando um agente basta

Cada agente acrescenta chamadas de modelo, checkpoints e handoffs, e mais um ponto de falha. Divida o trabalho quando as partes forem independentes, quando um único contexto ficaria grande demais, ou quando uma parte precisar de outras ferramentas ou instruções. Caso contrário, comece com um agente.

Neste lab, a equipe de suporte da Halden cuida do chamado T-48213, de um cliente cobrado duas vezes pelo pedido 48213. Execute dos dois jeitos e compare o que os traces medem.

## Experimente: Um céu de agentes

Três passos: execute o trabalho como supervisor e veja cada superstep salvar um checkpoint, abra um subgrafo na árvore de execuções, depois execute o mesmo trabalho com um agente e compare.

https://agenticademy.org/pt/paths/agent-systems/05-orchestration

## Verificação

results tem um reducer que concatena listas. O que o grafo devolve?

Código:

```
new StateGraph(State)
  .addNode("lookup", s => ({ results: [s.id * 10] }))
  .addConditionalEdges(START, () => [new Send("lookup", { id: 1 }), new Send("lookup", { id: 2 })], ["lookup"])
  .addEdge("lookup", END)
  .compile()
  .invoke({});
```

- **✓** { results: [10, 20] }: duas execuções de lookup em um superstep.
- { results: [20] }: o segundo Send sobrescreve o primeiro.
- { results: [10] }: só o primeiro Send roda.
- Lança um erro: um nó não pode receber dois Send.

**Por quê:** Cada Send inicia sua própria execução de lookup com sua própria entrada ({ id: 1 } e { id: 2 }). As duas rodam no mesmo superstep, e o reducer concatena as atualizações delas.

## O que você já sabe fazer

- Criar um supervisor que roteia para agentes especialistas com chamadas de ferramenta de handoff.
- Fazer fan-out com Send e mesclar resultados com um reducer.
- Usar um grafo compilado como subgrafo, e dizer quando um agente basta.
- Executar um grafo em uma thread e ler o último checkpoint dela com getState.

---

[Voltar para Sistemas de agentes e padrões](README.md) · [Próximo: Engenharia de contexto e recuperação](06-context-engineering.md)
