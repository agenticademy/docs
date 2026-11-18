# Construa seus primeiros agentes

Iniciante · Para desenvolvedores · 8 capítulos · 152 min

Para desenvolvedores começando com agentes. Você constrói um pequeno agente de suporte passo a passo: uma chamada de modelo, mensagens e estado, um grafo, ferramentas, o loop do agente, callbacks com aprovação humana e guardrails. Todo capítulo roda as bibliotecas reais no seu navegador com um modelo roteirizado, então você não precisa de chave; pode conectar seu próprio modelo quando quiser.

## Antes de começar

Um pouco de JavaScript: funções, objetos, async/await e instalação de pacotes com npm.

## No fim você sabe

- Escolher os pacotes do LangChain.js e do LangGraph.js para um agente e saber o que cada um faz.
- Chamar um modelo de chat e trabalhar com mensagens e com o estado do grafo.
- Construir um StateGraph com nós, arestas e roteamento condicional.
- Definir ferramentas com schemas zod e executar um agente ReAct mínimo.
- Pausar uma execução para aprovação humana com checkpoints e interruptBefore, e registrá-la com callbacks.
- Adicionar guardrails: verificações de entrada e de saída, listas de ferramentas permitidas e limites de passos.

## Capítulos

1. [As bibliotecas, e o que roda aqui](01-libraries-and-setup.md): Quais pacotes do LangChain.js e do LangGraph.js um agente precisa, o que cada um faz e como rodam em um navegador.
2. [Uma chamada de modelo](02-one-model-call.md): Chamar um modelo de chat com LangChain.js, ler o AIMessage que ele devolve e usar sua própria chave com segurança.
3. [Mensagens e estado](03-messages-and-state.md): Os quatro tipos de mensagem, por que a conversa inteira é enviada toda vez e como o LangGraph mescla o estado com reducers.
4. [Seu primeiro grafo](04-your-first-graph.md): Construir um StateGraph com nós e arestas, rotear com uma aresta condicional e executá-lo passo a passo.
5. [Ferramentas e chamadas de ferramenta](05-tools-and-tool-calls.md): Definir ferramentas com zod, vinculá-las a um modelo e tratar as chamadas de ferramenta que o modelo faz.
6. [O loop do agente: um agente ReAct mínimo](06-the-agent-loop.md): Um nó de modelo, um nó de ferramentas e uma aresta condicional: o loop inteiro do agente como um grafo LangGraph.
7. [Callbacks e aprovação humana](07-callbacks-and-approval.md): Acompanhar uma execução com callbacks e pausá-la antes de uma ferramenta arriscada para que uma pessoa a aprove ou rejeite.
8. [Guardrails](08-guardrails.md): Verificações em código ao redor do modelo: verificações de entrada e de saída, listas de ferramentas permitidas e limites de passos, em camadas.

https://agenticademy.org/pt/paths/first-agents
