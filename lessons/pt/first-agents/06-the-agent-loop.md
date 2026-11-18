# O loop do agente: um agente ReAct mínimo

Construa seus primeiros agentes · Capítulo 6 de 8 · 20 minutos

Um nó de modelo, um nó de ferramentas e uma aresta condicional: o loop inteiro do agente como um grafo LangGraph.

## Objetivos

- Avance o agente um superstep por vez até ele responder.
- Conte quantas vezes o modelo rodou.
- Coloque a execução em ordem.

## A leitura

### Raciocinar, agir, observar, repetir

ReAct (reason and act, raciocinar e agir) é um padrão para agentes: o modelo lê a conversa e pede ferramentas ou responde. Quando ele pede, seu código executa as ferramentas e adiciona os resultados à conversa, e o modelo lê de novo. O loop termina quando o modelo responde sem pedir uma ferramenta.

### O loop como um grafo

No LangGraph o loop são dois nós sobre MessagesAnnotation, o estado que guarda a lista de mensagens. O nó agent chama o modelo, com as ferramentas vinculadas, sobre state.messages e devolve sua resposta. O nó tools é o ToolNode de @langchain/langgraph/prebuilt: ele executa cada chamada de ferramenta do último AIMessage e adiciona um ToolMessage por chamada. Argumentos inválidos viram ToolMessages de erro, como no capítulo 5.

toolsCondition é o router depois do nó agent: ele devolve "tools" quando a última mensagem tem tool\_calls e END quando não tem. Uma aresta leva de tools de volta a agent. Esse é o agente inteiro, cerca de 25 linhas.

O LangGraph executa um grafo em supersteps: em cada um, os nós da vez rodam uma vez. Neste grafo só um nó é da vez de cada vez, então cada superstep é a execução de um nó, e graph.stream(input, { streamMode: "updates" }) entrega uma atualização por superstep. Você pode ver o loop girar.

### Pronto, e com limite

createReactAgent, em @langchain/langgraph/prebuilt, constrói o mesmo grafo; ele está obsoleto em favor de createAgent, do pacote langchain. Construa-o à mão uma vez e você vai saber o que esses atalhos fazem.

Um loop pode rodar para sempre se o modelo continuar pedindo ferramentas. recursionLimit, na config da execução, limita os supersteps (25 por padrão); quando ele é atingido, o LangGraph lança GraphRecursionError. Experimente um limite de 4 abaixo: esta execução precisa de 5 supersteps.

## Experimente: O loop, um passo por vez

Três passos, uma ação cada, e um quarto opcional: envie o chamado T-48213, avance o loop até o agente responder, conte as chamadas ao modelo e depois limite o loop com recursionLimit.

https://agenticademy.org/pt/paths/first-agents/06-the-agent-loop

## Verificação

Coloque em ordem a execução do chamado T-48213.

1. O nó agent envia a tarefa ao modelo.
2. O modelo pede get\_ticket.
3. O ToolNode executa get\_ticket e adiciona um ToolMessage.
4. O modelo o lê e pede list\_charges.
5. O ToolNode executa list\_charges e adiciona um ToolMessage.
6. O modelo responde sem chamadas de ferramenta.
7. toolsCondition encaminha para END.

**Por quê:** Cada turno do modelo pede ferramentas, o que volta pelo ToolNode, ou responde, o que termina a execução. O modelo rodou três vezes.

## O que você já sabe fazer

- Construir um agente ReAct como um grafo com ToolNode e toolsCondition.
- Acompanhar uma execução mensagem por mensagem com streamMode "updates".
- Explicar por que todo loop de agente precisa de um limite de recursão.

---

[Voltar para Construa seus primeiros agentes](README.md) · [Próximo: Callbacks e aprovação humana](07-callbacks-and-approval.md)
