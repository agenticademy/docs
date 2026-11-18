# Sistemas de agentes e padrões

Intermediário · Para desenvolvedores · 8 capítulos · 195 min

Para desenvolvedores que já sabem criar um único agente. Você conecta agentes a ferramentas, a outros agentes, a interfaces e a pagamentos com os padrões abertos, orquestra vários agentes em um grafo, decide o que entra em cada contexto, faz o trace das execuções e as testa contra ataques. As bibliotecas dos protocolos rodam de verdade no seu navegador; você pode reter, descartar e adulterar cada mensagem.

## Antes de começar

A trilha Crie seus primeiros agentes, ou o mesmo conhecimento: mensagens e ferramentas do LangChain.js, grafos do LangGraph e o loop do agente.

## No fim você sabe

- Conectar um agente a um servidor MCP com o protocolo 2026-07-28, incluindo pedidos de resposta a uma pessoa.
- Delegar trabalho a outro agente com tarefas A2A e Agent Cards.
- Transmitir o trabalho de um agente a uma interface com AG-UI e descrever uma UI segura com A2UI.
- Explicar como UCP e AP2 vinculam a compra de um agente ao consentimento assinado de uma pessoa.
- Orquestrar agentes com supervisores, fan-out com Send e subgrafos.
- Fazer engenharia de contexto com busca, remoção e subagentes, e fazer o trace de execuções no formato do LangSmith.
- Testar um sistema de agentes contra prompt injection e adulteração, usando os riscos agênticos da OWASP como mapa.
- Ler uma linha do tempo de supersteps e uma árvore de execuções.
- Adicionar a um agente um limite de chamadas, um limite de ferramentas ou uma proteção como middleware.

## Capítulos

1. [MCP: ferramentas de um servidor](01-mcp.md): Como um agente encontra e chama ferramentas em um servidor MCP, e como o protocolo 2026-07-28 pergunta a uma pessoa antes de uma chamada arriscada.
2. [A2A: agentes que delegam a agentes](02-a2a.md): Como um agente encontra outro pelo seu Agent Card e lhe entrega uma tarefa que pode acompanhar até o fim.
3. [Agentes e interfaces: AG-UI e A2UI](03-agents-and-uis.md): Transmitir o trabalho de um agente a um app com AG-UI, e deixar um agente descrever componentes de interface seguros com A2UI.
4. [Agentes que pagam: UCP e AP2](04-agent-commerce.md): Como um agente faz o checkout com um comerciante via UCP, e como os mandatos AP2 vinculam o pagamento ao que a pessoa assinou.
5. [Orquestração: supervisores, fan-out, subgrafos](05-orchestration.md): Padrões para vários agentes em um grafo LangGraph: um supervisor que roteia, Send para trabalho em paralelo, subgrafos, e o checkpoint que cada superstep salva.
6. [Engenharia de contexto e recuperação](06-context-engineering.md): Escolher o que cada chamada de modelo lê: grep ou busca vetorial, descarregar resultados grandes, um primeiro olhar sobre subagentes, e uma memória que dura entre threads.
7. [Observabilidade: traces e execuções](07-observability.md): Registrar cada chamada de modelo, chamada de ferramenta e passo como uma árvore de execuções, e lê-la para achar o que está lento ou quebrado.
8. [Avaliação e segurança](08-evaluation-and-safety.md): Testar um sistema de agentes com casos e verificações, atacá-lo com injeção e adulteração, e mapear as falhas nos riscos agênticos da OWASP.

https://agenticademy.org/pt/paths/agent-systems
