# Callbacks e aprovação humana

Construa seus primeiros agentes · Capítulo 7 de 8 · 22 minutos

Acompanhar uma execução com callbacks e pausá-la antes de uma ferramenta arriscada para que uma pessoa a aprove ou rejeite.

## Objetivos

- Anexe um handler de callbacks e acompanhe seus eventos.
- Pause antes da ferramenta de reembolso e depois aprove-a.
- Rejeite um reembolso e veja como o agente lida com isso.
- Passe na verificação.

## A leitura

### Callbacks observam

Um handler de callbacks é um objeto cujos métodos o LangChain chama durante uma execução: uma subclasse de BaseCallbackHandler com métodos como handleChatModelStart, handleLLMEnd, handleToolStart e handleToolEnd. Você o passa em config.callbacks, e ele chega a toda chamada aninhada que recebe essa config. Use-o para logs, métricas e traces. Um callback só observa: o LangChain ignora o que seus métodos retornam e, por padrão, só registra um erro lançado dentro de um deles, então ele não pode mudar nem parar um passo.

### Breakpoints pausam

Para parar uma execução antes de um passo, o LangGraph precisa salvar o estado e voltar a ele. Isso é um checkpointer: compile({ checkpointer: new MemorySaver() }) salva um checkpoint depois de cada superstep, sob o thread\_id em config.configurable. Adicione interruptBefore: ["approval\_tools"] e a execução para antes desse nó.

Enquanto ela está pausada, (await graph.getState(config)).next diz o que está esperando, e o último AIMessage guarda a chamada pendente. Para aprovar, retome com graph.invoke(null, config): uma entrada null significa “continue a partir do checkpoint”. Para rejeitar, chame antes graph.updateState(config, { messages: [toolMessage] }, "approval\_tools"): seu ToolMessage responde à chamada como se o nó tivesse rodado, então o nó é pulado e o modelo lê sua recusa.

Dê às ferramentas arriscadas um nó próprio, como aqui com create\_refund, para que a barreira pare só elas.

### No Node, e em outros lugares

No Node, o createAgent do pacote langchain oferece hooks de middleware (beforeModel, afterModel, wrapModelCall, wrapToolCall) e humanInTheLoopMiddleware, que pausa com interrupt(). interrupt() precisa de AsyncLocalStorage, então em um navegador você usa breakpoints, como aqui. Agentes de programação usam a mesma ideia: hooks que rodam antes de uma chamada de ferramenta e podem negá-la.

## Experimente: Uma barreira antes do reembolso

Quatro passos, uma ação em cada, e um quinto opcional: acompanhe os eventos de callback de uma execução, ponha uma barreira antes do reembolso, aprove uma execução e depois rejeite a seguinte.

https://agenticademy.org/pt/paths/first-agents/07-callbacks-and-approval

## Verificação

### Pergunta 1 de 3

Do que interruptBefore precisa para funcionar?

- **✓** Um checkpointer, e um thread\_id na config.
- Um handler de callbacks.
- Um modelo real.

**Por quê:** Pausar significa salvar o estado e voltar a ele. O checkpointer o guarda sob o id da thread.

### Pergunta 2 de 3

A execução está pausada antes de approval\_tools. Como você a continua como estava?

- Chamar invoke de novo com a entrada original.
- **✓** graph.invoke(null, config) com o mesmo thread\_id.
- Começar uma thread nova.

**Por quê:** Uma entrada null diz ao LangGraph para retomar a partir do checkpoint salvo em vez de começar uma nova execução.

### Pergunta 3 de 3

O handleToolStart de um handler de callbacks devolve false quando vê create\_refund. O que acontece?

- O reembolso é bloqueado.
- **✓** Nada muda: o LangChain ignora o valor devolvido, então o reembolso roda. Para parar um passo, pause o grafo ou proteja a ferramenta.
- A execução pausa antes de approval\_tools.

**Por quê:** Callbacks servem para observar e registrar. O controle fica no grafo: um breakpoint com updateState, como aqui, ou uma verificação dentro do nó de ferramentas.

## O que você já sabe fazer

- Registrar os eventos de modelo e de ferramenta de uma execução com um handler de callbacks.
- Pausar um grafo antes de um nó com um checkpointer e interruptBefore.
- Retomar ou rejeitar um passo pendente com getState, updateState e invoke(null).

---

[Voltar para Construa seus primeiros agentes](README.md) · [Próximo: Guardrails](08-guardrails.md)
