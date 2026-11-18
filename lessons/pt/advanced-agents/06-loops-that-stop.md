# Loops que param

Agentes avançados · Capítulo 6 de 10 · 20 minutos

Um loop precisa de paradas que valem não importa o que o modelo faça: um limite de recursão, um orçamento de chamadas, um detector de chamadas repetidas e uma pessoa que pode dizer não.

## Objetivos

- Execute um agente que repete uma chamada de ferramenta até que GraphRecursionError o pare.
- Arme um orçamento de chamadas e um detector de repetições, e veja qual dispara primeiro.
- Pause antes de uma ferramenta arriscada e recuse a chamada.
- Passe na verificação.

## A leitura

### Um loop precisa de paradas que o modelo não escolhe

Um loop de agente termina quando o modelo responde sem pedir uma ferramenta. Um turno é uma chamada ao modelo mais as ferramentas que ele pede. Se o modelo continua pedindo, por exemplo para conferir um status que nunca muda, cada turno é mais uma chamada paga ao modelo e nada de novo é aprendido. Por isso você adiciona paradas que valem não importa o que o modelo faça.

### O teto do LangGraph: recursionLimit

Em um loop como este, cada execução de um nó é um superstep. recursionLimit, na config da execução, põe um teto nos supersteps, 25 por padrão. Passado esse teto, o LangGraph.js lança GraphRecursionError, com lc\_error\_code GRAPH\_RECURSION\_LIMIT. É uma rede de segurança: conta supersteps, não turnos, para tarde e, sem um checkpointer, o estado da execução se perde junto com o erro.

### Paradas como middleware

Middleware são hooks que o createAgent do LangChain 1.6 executa em volta da chamada ao modelo; os hooks beforeModel e afterModel viram, cada um, um nó do grafo. modelCallLimitMiddleware é um orçamento em chamadas ao modelo: runLimit conta uma execução, threadLimit uma thread inteira, e exitBehavior 'end' encerra a execução com uma mensagem. toolCallLimitMiddleware conta as chamadas a uma ferramenta ou a todas. Um orçamento em tokens ou em dinheiro funciona do mesmo jeito: contar, comparar, parar. Este site recria os dois, como modelCallLimit e toolCallLimit; o laboratório usa modelCallLimit.

Um detector de repetições pega cedo um loop travado: a mesma ferramenta com os mesmos argumentos, threshold vezes nas últimas chamadas. O repeatDetector daqui é próprio deste site; o LangChain não tem um embutido. Com exitBehavior 'end', ele encerra a execução; com 'continue', recusa a chamada e diz ao modelo para tentar outra coisa. Aqui cada hook também grava um evento de trace, para você ver qual parada disparou.

### Uma pessoa como parada

Algumas chamadas deveriam esperar um sim, como enviar dinheiro. interruptBefore pausa o grafo antes de um nó, e um checkpointer guarda o estado pausado. O breakpoint para antes de cada execução desse nó, então o seu código escolhe as chamadas que precisam de uma pessoa e as mostra. Para recusar, updateState responde à chamada com uma ToolMessage como se o nó tools tivesse rodado; invocar de novo com uma entrada null retoma a execução. No Node, humanInTheLoopMiddleware faz isso com interrupt(), que precisa de AsyncLocalStorage; os navegadores não têm, então esta página usa o breakpoint.

## Experimente: Três jeitos de parar um loop

Um agente roteirizado confere um reembolso que continua pendente. Pare-o com recursionLimit, depois com um orçamento e um detector de repetições, depois com uma pessoa antes da ferramenta arriscada.

https://agenticademy.org/pt/paths/advanced-agents/06-loops-that-stop

## Verificação

O modelo pede check\_refund({ order: '4471' }) em todo turno e nunca responde. Qual parada encerra esta execução?

Código:

```
const agent = createAgent({
  model,
  tools: [checkRefund],
  middleware: [
    modelCallLimit({
      runLimit: 3,
      exitBehavior: 'end',
    }),
    repeatDetector({
      window: 6,
      threshold: 4,
      exitBehavior: 'end',
    }),
  ],
});
// recursionLimit: 25 (the default)
await agent.graph.invoke({
  messages: [task],
});
```

- GraphRecursionError, no superstep 25.
- O detector de repetições, na quarta chamada idêntica.
- **✓** O orçamento de chamadas, antes da quarta chamada ao modelo.
- Nenhuma: uma execução só termina quando o modelo responde.

**Por quê:** O orçamento de chamadas permite três chamadas ao modelo; antes da quarta, modelCallLimit encerra a execução. Então o detector de repetições nunca vê uma quarta chamada idêntica. Aqui cada turno leva cinco supersteps, então a execução termina no superstep 16, bem antes dos 25 do recursionLimit. E uma parada nunca espera o modelo: é para isso que ela serve.

## O que você já sabe fazer

- Definir recursionLimit como rede de segurança e capturar GraphRecursionError.
- Adicionar um orçamento de chamadas e um detector de repetições como middleware, e ler no evento de trace qual deles disparou.
- Pausar antes de uma ferramenta arriscada com interruptBefore, e recusar a chamada com updateState.

---

[Voltar para Agentes avançados](README.md) · [Próximo: Loops de planejamento](07-planning-loops.md)
