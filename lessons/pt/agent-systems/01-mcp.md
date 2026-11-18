# MCP: ferramentas de um servidor

Sistemas de agentes e padrões · Capítulo 1 de 8 · 25 minutos

Como um agente encontra e chama ferramentas em um servidor MCP, e como o protocolo 2026-07-28 pergunta a uma pessoa antes de uma chamada arriscada.

## Objetivos

- Execute o cenário até o reembolso ser emitido.
- Responda você mesmo à confirmação do helpdesk.
- Adultere uma mensagem e veja o SDK real rejeitá-la.
- Passe na verificação.

## A leitura

### Hosts, clientes e servidores

O Model Context Protocol (MCP) é um padrão aberto para conectar aplicações de IA a ferramentas e dados. A aplicação, chamada de host, roda um cliente MCP para cada servidor que usa. Um servidor oferece ferramentas (ações que o modelo pode chamar), recursos (dados que a aplicação pode ler) e prompts (modelos que uma pessoa pode escolher). As mensagens são JSON-RPC 2.0, via HTTP ou via a entrada e saída padrão de um processo local (stdio).

No agente da trilha anterior, as ferramentas eram funções no seu código. Com MCP, elas vivem em um servidor que outra pessoa pode escrever e rodar, e qualquer host MCP pode usá-las.

### O que a 2026-07-28 mudou

Este capítulo usa a versão 2026-07-28 do protocolo com o SDK oficial de TypeScript v2. Você vai ver três mudanças no fio.

Descoberta: o cliente chama server/discover para conhecer as versões e capacidades do servidor. Servidores nas versões de 2025 respondem a initialize, e o SDK recorre a ele.

Uma versão em cada requisição: cada requisição leva a versão do protocolo em \_meta, então o servidor não guarda sessão, e qualquer instância dele pode responder a qualquer requisição.

Resposta necessária: quando uma ferramenta precisa da resposta de uma pessoa, o servidor encerra a requisição com input\_required, a pergunta e um requestState assinado. O cliente pergunta à pessoa e chama a ferramenta de novo com a resposta e o mesmo requestState. O servidor confere a assinatura, então a nova tentativa precisa corresponder ao que a pessoa viu.

### Ferramentas são um contrato

O modelo vê apenas o nome, a descrição e o JSON Schema de uma ferramenta. Anotações como readOnlyHint e destructiveHint são dicas do servidor: um cliente confia nelas só na medida em que confia nesse servidor. O reembolso neste lab é seguro porque o próprio servidor pede uma confirmação e a confere, não porque o modelo se comporta bem.

### O que você faz aqui

O agente de suporte da Halden, um agente LangChain com o cliente do SDK, e o helpdesk da Halden, o servidor do SDK, rodam nesta página. Execute o cenário e responda você mesmo à pergunta do reembolso. Depois ative Interceptar, retenha uma mensagem e adultere-a: o código real do SDK a rejeita.

## Experimente: O fio: um agente e um helpdesk MCP

Quatro passos, uma ação cada, e um quinto opcional. O diagrama abaixo dos passos tem uma coluna por participante e uma linha por mensagem, na ordem em que é enviada, de cima para baixo.

https://agenticademy.org/pt/paths/agent-systems/01-mcp

## Verificação

### Pergunta 1 de 4

No MCP, quem roda o cliente?

- O servidor.
- **✓** A aplicação host, por exemplo o agente de suporte.
- O provedor do modelo.

**Por quê:** O host, como um agente ou uma IDE, roda um cliente para cada servidor ao qual se conecta. O modelo nunca fala diretamente com o servidor; quem fala é o host, quando o modelo pede uma ferramenta.

### Pergunta 2 de 4

O helpdesk precisa do OK do líder de suporte antes de um reembolso. O que um servidor 2026-07-28 envia?

- Uma nova requisição do servidor para o cliente, pedindo uma resposta.
- **✓** Um resultado com input\_required, a pergunta e um requestState assinado, que encerra a requisição.
- O erro -32602.

**Por quê:** O servidor encerra a requisição em vez de esperar com uma conexão aberta. O cliente pergunta à pessoa e tenta de novo com a resposta e o mesmo requestState, então qualquer instância do servidor pode terminar o trabalho.

### Pergunta 3 de 4

Na nova tentativa, você aumenta o valor do reembolso. Por que o servidor a rejeita?

- O modelo percebe a mudança.
- **✓** Os argumentos não correspondem mais ao requestState assinado, que foi emitido para o que a pessoa aprovou.
- O MCP não permite valores nos argumentos.

**Por quê:** O requestState é assinado pelo servidor (com HMAC aqui). O servidor lê de volta o pedido e o valor aprovados e recusa qualquer chamada diferente.

### Pergunta 4 de 4

Um servidor marca uma ferramenta com readOnlyHint: true. O que o cliente deve fazer?

- Executá-la sem nunca perguntar a ninguém, já que ela não pode mudar nada.
- **✓** Tratar como uma dica, confiável só na medida em que o cliente confia nesse servidor.
- Recusar-se a executá-la.

**Por quê:** Anotações descrevem o que o servidor afirma. Um servidor malicioso ou com bugs pode rotular mal uma ferramenta, então é a política do próprio cliente que decide o que precisa de aprovação.

## O que você já sabe fazer

- Descrever um host, um cliente e um servidor MCP, e as três coisas que um servidor oferece.
- Seguir o fluxo 2026-07-28: server/discover, tools/list, tools/call, input\_required e a nova tentativa.
- Explicar por que um requestState assinado mantém uma confirmação vinculada ao que a pessoa viu.
- Tratar anotações de ferramentas como dicas, não como garantias.

---

[Voltar para Sistemas de agentes e padrões](README.md) · [Próximo: A2A: agentes que delegam a agentes](02-a2a.md)
