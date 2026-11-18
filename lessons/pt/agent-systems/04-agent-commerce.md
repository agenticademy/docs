# Agentes que pagam: UCP e AP2

Sistemas de agentes e padrões · Capítulo 4 de 8 · 25 minutos

Como um agente faz o checkout com um comerciante via UCP, e como os mandatos AP2 vinculam o pagamento ao que a pessoa assinou.

## Objetivos

- Crie e conclua uma sessão de checkout.
- Assine os mandatos no celular.
- Mude o valor depois da assinatura e veja a verificação falhar.
- Passe na verificação.

## A leitura

### UCP: o checkout

O UCP (Universal Commerce Protocol, versão 2026-08-25) é como um agente de compras, a plataforma, compra de um comerciante, o negócio. O negócio publica um perfil em /.well-known/ucp: sua versão, suas capacidades, como checkout, seus handlers de pagamento e seu endpoint REST. O agente o lê, depois cria uma sessão de checkout com POST /checkout-sessions, a atualiza com PUT até o status ser ready\_for\_complete e termina com POST …/complete.

Cada chamada leva um cabeçalho UCP-Agent que aponta para o perfil da própria plataforma, para que o negócio possa conferir o que a plataforma suporta. Cada escrita leva uma Idempotency-Key: se um timeout faz o agente enviar a mesma requisição de novo, o negócio responde com o primeiro resultado em vez de agir duas vezes. Uma cobrança em dobro como a do pedido 48213 da Halden é exatamente o que ela evita.

### AP2: a assinatura da pessoa

O UCP sozinho confia em quem tiver a sessão. O AP2 (Agent Payments Protocol) acrescenta a prova de consentimento. O provedor do agente emite dois mandatos como credenciais verificáveis SD-JWT: um mandato de checkout com um hash do checkout exato que a Halden assinou, e um mandato de pagamento com o valor, o beneficiário e o cartão. A pessoa os revisa em uma tela na qual o agente não pode desenhar, a carteira do seu celular, e a chave do celular assina um JWT de key binding em cada um.

A Halden confere o mandato de checkout com o checkout que assinou. A carteira de cartões confere o mandato de pagamento antes de emitir um token de pagamento para aquele valor, e o processador confere o token antes de cobrar. Mude o carrinho ou o valor depois que a pessoa assinou e um hash ou uma assinatura deixa de corresponder, diga o agente o que disser.

Aqui a pessoa está presente. No modo autônomo do AP2, não está: ela aprova antes mandatos abertos, com limites como um preço máximo, e o agente assina os mandatos finais dentro deles.

### ANP, em duas frases

O ANP (Agent Network Protocol) é para agentes que se encontram na web aberta: cada agente tem um identificador did:wba cujo documento DID, servido pelo seu próprio domínio, lista suas chaves públicas. Cada requisição é assinada com HTTP Message Signatures, então o outro lado sabe qual agente a enviou sem precisar de uma conta. Os labs de ANP do passo Vá além o executam.

### O que você faz aqui

Um agente de compras compra a barraca Ridge 2 da Halden. Acompanhe o checkout, aprove no celular, depois execute de novo com Interceptar ativado e aumente o valor depois da assinatura.

## Experimente: Uma cadeia de assinaturas

Cinco passos, depois dois opcionais. Abaixo dos passos, a cadeia acrescenta cada elo da compra quando a compra chega a ele, com quem o confere. Abaixo da cadeia, o lab desenha cada mensagem como uma linha, de cima para baixo.

https://agenticademy.org/pt/paths/agent-systems/04-agent-commerce

## Verificação

### Pergunta 1 de 4

O que um mandato de checkout do AP2 vincula?

- O nome do agente.
- **✓** Um hash do checkout exato, com seu carrinho e total, que a pessoa aprovou.
- O logotipo do comerciante.

**Por quê:** O mandato é a prova assinada do que a pessoa concordou em comprar. Um checkout diferente tem um hash diferente, então deixa de corresponder.

### Pergunta 2 de 4

O agente aumenta o valor depois que a pessoa assinou. Onde isso falha?

- Em lugar nenhum, se o agente explicar o motivo.
- **✓** Na verificação: a carteira de cartões, a Halden ou o processador descobre que uma assinatura ou um hash não corresponde mais.
- Só no banco, dias depois.

**Por quê:** Cada parte confere as assinaturas com os valores que tem à frente. O agente não consegue convencer um hash.

### Pergunta 3 de 4

O que a Idempotency-Key do UCP evita?

- Prompt injection.
- **✓** Que uma requisição repetida crie um segundo pedido ou uma segunda cobrança.
- Cartões vencidos.

**Por quê:** Uma nova tentativa da rede repete uma requisição. Com a mesma chave, o negócio devolve o primeiro resultado em vez de agir duas vezes: a cobrança em dobro da história da Halden é exatamente o que ela evita.

### Pergunta 4 de 4

Por que a pessoa aprova na carteira do celular e não no chat do agente?

- Celulares têm telas maiores.
- **✓** O agente não pode desenhar na tela da carteira, e só o celular tem a chave que assina os mandatos.
- O UCP exige um celular.

**Por quê:** O AP2 chama isso de superfície confiável. A pessoa vê os mandatos como foram emitidos, e a assinatura vem de uma chave que o agente nunca tem.

## O que você já sabe fazer

- Acompanhar uma sessão de checkout UCP do perfil até completed.
- Explicar como os mandatos AP2 vinculam um pagamento ao consentimento assinado de uma pessoa.
- Usar chaves de idempotência para tornar as novas tentativas seguras.
- Dizer em duas frases o que o did:wba e as requisições assinadas do ANP acrescentam.

---

[Voltar para Sistemas de agentes e padrões](README.md) · [Próximo: Orquestração: supervisores, fan-out, subgrafos](05-orchestration.md)
