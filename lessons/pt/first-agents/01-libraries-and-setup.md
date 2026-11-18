# As bibliotecas, e o que roda aqui

Construa seus primeiros agentes · Capítulo 1 de 8 · 15 minutos

Quais pacotes do LangChain.js e do LangGraph.js um agente precisa, o que cada um faz e como rodam em um navegador.

## Objetivos

- Abra os seis pacotes no mapa e leia o que cada um oferece.
- Execute o hello-world nesta página e leia a resposta.
- Passe na verificação.

## A leitura

### Os pacotes

LangChain.js e LangGraph.js são bibliotecas JavaScript de código aberto para construir com modelos de linguagem. Elas vêm em pacotes separados, então você instala só o que usa.

@langchain/core reúne as peças comuns: mensagens, a interface de modelo de chat, ferramentas e callbacks. @langchain/langgraph é o runtime para agentes como grafos: estado, nós, arestas, checkpoints e pausas para aprovação. Um pacote de provedor, como @langchain/anthropic ou @langchain/openai, oferece a classe de modelo de chat para a API de um provedor; a mesma classe ChatOpenAI também conversa com serviços compatíveis com OpenAI, como OpenRouter, Hugging Face e NVIDIA. zod descreve e valida a entrada das suas ferramentas; o LangChain transforma um schema zod no JSON Schema que o modelo lê. langsmith é opcional e registra traces de cada execução.

### Por que uma biblioteca de grafos

Você pode escrever um loop de agente à mão: chamar o modelo, executar as ferramentas que ele pede, repetir. O capítulo 6 constrói esse loop como um grafo de dois nós. O LangGraph adiciona o que falta a um loop escrito à mão: estado salvo depois de cada passo, para que uma execução possa pausar para uma pessoa e continuar depois; ramos que rodam em paralelo; e uma estrutura que você pode inspecionar e desenhar.

O pacote langchain é construído sobre o LangGraph e oferece createAgent, um agente pronto com middleware. Esta trilha primeiro constrói o grafo à mão, para que depois você saiba o que esses atalhos fazem por você.

### Instalar

No seu próprio projeto: npm install @langchain/core @langchain/langgraph @langchain/anthropic zod. Esta trilha usa @langchain/core 1.2.18, @langchain/langgraph 1.5.1 e zod 4.6.7. Fixe versões exatas em projetos reais: essas bibliotecas mudam rápido.

### O que muda em um navegador

Tudo nesta trilha roda no seu navegador, então importa o LangGraph de @langchain/langgraph/web. O ponto de entrada principal repassa a configuração para chamadas aninhadas com AsyncLocalStorage, que os navegadores não têm. Com /web, você passa config explicitamente para chamadas aninhadas e pausa execuções com breakpoints (interruptBefore) em vez de interrupt(). O resto da API é igual.

Por padrão o modelo é roteirizado, então você não precisa de chave e toda execução dá o mesmo resultado. Neste capítulo é o FakeListChatModel do próprio LangChain, que devolve as respostas que você lhe dá. A partir do capítulo 2 você pode conectar um modelo real com a sua própria chave.

## Experimente: Os pacotes

Abra cada pacote no mapa para ver o que ele oferece, a versão usada aqui e como importá-lo. Depois execute o hello-world.

https://agenticademy.org/pt/paths/first-agents/01-libraries-and-setup

## Verificação

### Pergunta 1 de 4

Qual pacote define HumanMessage, AIMessage e tool()?

- @langchain/langgraph
- **✓** @langchain/core
- @langchain/anthropic
- zod

**Por quê:** @langchain/core reúne as peças comuns. O LangGraph e os pacotes de provedor as importam de lá.

### Pergunta 2 de 4

Você roda um grafo LangGraph em um navegador. De onde importa StateGraph?

- @langchain/langgraph
- langchain
- **✓** @langchain/langgraph/web

**Por quê:** O ponto de entrada /web não depende de AsyncLocalStorage, que falta nos navegadores. Você então passa config explicitamente para chamadas aninhadas.

### Pergunta 3 de 4

O que o zod faz em um agente LangChain?

- **✓** Ele descreve e valida as entradas das ferramentas; o LangChain envia o schema ao modelo como JSON Schema.
- Ele chama o modelo.
- Ele salva checkpoints entre os passos.

**Por quê:** O schema serve duas vezes: o modelo o lê para saber quais argumentos enviar, e seu código o usa para rejeitar argumentos ruins.

### Pergunta 4 de 4

Por que o hello-world usa FakeListChatModel?

- Ele raciocina melhor que modelos reais.
- Modelos reais não podem ser chamados de um navegador.
- **✓** Ele devolve respostas fixas, então o código roda sem chave e sem rede, sempre do mesmo jeito.

**Por quê:** Um modelo falso torna o código testável e gratuito de rodar. Modelos reais podem ser chamados de um navegador com uma chave, como mostra o capítulo 2.

## O que você já sabe fazer

- Escolher os pacotes para um agente LangGraph.js e dizer o que cada um faz.
- Instalá-los com versões fixadas.
- Explicar o que muda quando o LangGraph roda em um navegador.
- Rodar código LangChain com um modelo falso, sem precisar de chave.

---

[Voltar para Construa seus primeiros agentes](README.md) · [Próximo: Uma chamada de modelo](02-one-model-call.md)
