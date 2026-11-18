# Loops que resistem

Agentes avançados · Capítulo 5 de 10 · 20 minutos

Políticas de nova tentativa, timeouts, handlers de erro e checkpoints mantêm uma execução longa de pé quando uma ferramenta falha ou o processo para.

## Objetivos

- Injete uma ferramenta instável e observe as novas tentativas.
- Envie a falha para uma etapa de compensação.
- Pare a execução no meio e retome-a a partir do checkpoint.
- Preveja as tentativas e o caminho.

## A leitura

### Novas tentativas e timeouts

Uma chamada de ferramenta pode falhar por um momento: um 503 de um serviço ocupado, ou nenhuma resposta. No LangGraph.js 1.5.1, um nó pode ter uma retryPolicy: maxAttempts (a primeira tentativa conta), initialInterval e backoffFactor para as esperas entre tentativas, e retryOn para escolher os erros que valem outra tentativa. Um timeout transforma uma chamada travada em uma tentativa que falhou. runTimeout limita o tempo total de uma tentativa; idleTimeout limita o tempo sem progresso, como uma escrita de estado, um evento de callback ou uma chamada a heartbeat(). Os dois lançam NodeTimeoutError, que a política de nova tentativa trata como qualquer outro erro.

### Handlers de erro e compensação

Quando as tentativas acabam, o errorHandler do nó, adicionado no LangGraph.js 1.4, roda em vez de fazer a execução falhar. Ele recebe o estado de entrada do nó e um NodeError, que contém o nome do nó e o erro, e retorna uma atualização de estado ou um Command({ goto }). Enviar a execução para uma compensação é o padrão saga: uma etapa que desfaz o trabalho feito antes da falha, como liberar um quarto que foi reservado mas nunca pago.

### Parar e retomar

Com um checkpointer, cada superstep termina salvando um checkpoint na thread. Passe um RunControl na config da execução e chame requestDrain(): os nós que já estão rodando terminam, o checkpoint é salvo e a execução lança GraphDrained. Invocar de novo com entrada null no mesmo thread\_id começa no próximo nó; os nós que terminaram não rodam de novo. Com um checkpointer que salva em um banco de dados em vez da memória, isso funciona também a partir de um processo novo.

Uma queda não dá esse aviso. O nó que estava rodando perde suas escritas e roda de novo ao retomar, então uma chamada com efeitos colaterais deve levar uma chave de idempotência: uma chave que o serviço lembra, para que uma requisição repetida receba o primeiro resultado em vez de agir duas vezes.

### Neste lab

Um grafo de reservas: o modelo planeja a viagem, e depois reserve, charge e confirm chamam cada um um serviço. Os navegadores não têm AsyncLocalStorage, então interrupt() e o task() da API funcional não podem rodar aqui. O lab usa a API de grafos e passa a config para cada chamada à mão. O segundo processo é um grafo novo sobre o mesmo MemorySaver em memória, e puxar o plugue aborta o AbortSignal da execução.

## Experimente: Falhar, recuperar, retomar

Deixe uma ferramenta instável e observe as novas tentativas, derrube o serviço de pagamento e siga o caminho até a compensação, depois pare o processo e retome a thread.

https://agenticademy.org/pt/paths/advanced-agents/05-loops-that-survive

## Verificação

charge\_card lança um 503 em toda chamada. O que acontece quando isto roda?

Código:

```
const graph = new StateGraph(Trip)
  .addNode("reserve", reserve)
  .addNode("charge", charge, {
    retryPolicy: {
      maxAttempts: 2,
      initialInterval: 100,
    },
    errorHandler: () =>
      new Command({ goto: "release" }),
  })
  .addNode("release", release)
  .addEdge(START, "reserve")
  .addEdge("reserve", "charge")
  .addEdge("charge", END)
  .addEdge("release", END)
  .compile();

await graph.invoke(input);
```

- **✓** charge\_card é chamado duas vezes; depois o errorHandler envia a execução para release, e invoke retorna normalmente.
- charge\_card é chamado três vezes, a primeira tentativa e duas novas tentativas; depois release roda.
- charge\_card é chamado duas vezes; depois invoke lança o 503.
- O errorHandler roda depois do primeiro 503, e então charge é tentado de novo.

**Por quê:** maxAttempts conta todas as tentativas, incluindo a primeira, então charge\_card é chamado duas vezes, não três. O errorHandler só roda quando essas tentativas acabam, nunca entre elas. O Command dele envia a execução para release, que termina em END, então invoke retorna o estado. Um nó sem errorHandler faz invoke lançar o 503 quando as tentativas dele acabam.

## O que você já sabe fazer

- Dar a um nó uma retryPolicy e um timeout, e prever quantas tentativas ele faz.
- Enviar a última falha de um nó para uma etapa de compensação com errorHandler e Command.
- Parar uma execução com RunControl.requestDrain e retomá-la a partir do checkpoint sem repetir os nós que terminaram.
- Usar chaves de idempotência para que um nó que roda de novo depois de uma queda não aja duas vezes.

---

[Voltar para Agentes avançados](README.md) · [Próximo: Loops que param](06-loops-that-stop.md)
