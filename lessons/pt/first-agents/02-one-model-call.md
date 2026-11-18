# Uma chamada de modelo

Construa seus primeiros agentes · Capítulo 2 de 8 · 15 minutos

Chamar um modelo de chat com LangChain.js, ler o AIMessage que ele devolve e usar sua própria chave com segurança.

## Objetivos

- Execute a chamada com o modelo roteirizado.
- Encontre as contagens de tokens na resposta.
- Preveja o que o código imprime.

## A leitura

### Uma interface, muitos provedores

Cada pacote de provedor tem uma classe de modelo de chat, como ChatAnthropic ou ChatOpenAI. Elas compartilham uma interface: invoke(messages) envia uma lista de mensagens e devolve um AIMessage. Você também pode passar uma string simples, que o LangChain envolve em um HumanMessage. stream(messages) devolve a mesma resposta em pedaços à medida que chegam, para você mostrar texto enquanto o modelo ainda escreve.

As opções vão para o construtor: model (o nome do modelo naquele provedor), temperature (0 dá a redação mais previsível; valores mais altos variam mais) e maxTokens (a resposta mais longa pela qual você vai pagar). ChatOpenAI também aceita configuration.baseURL, então a mesma classe conversa com qualquer serviço compatível com OpenAI: OpenRouter, o roteador da Hugging Face ou o catálogo de APIs da NVIDIA.

### O que volta

O AIMessage tem content (o texto, para uma resposta simples), tool\_calls (vazio até o capítulo 5), usage\_metadata e response\_metadata. usage\_metadata conta tokens, os pedaços de texto que um modelo lê e escreve: input\_tokens para o que você enviou, output\_tokens para a resposta. Os provedores cobram por tokens, e a entrada cresce a cada mensagem que você envia, então este é o número a acompanhar. response\_metadata guarda o que o provedor acrescenta, como o nome do modelo e por que ele parou.

### Chaves

Uma chave de API é uma senha para uma conta paga. Mantenha-a fora do código e dos repositórios: leia-a de uma variável de ambiente na sua máquina ou servidor. Nesta página, uma chave que você conecta fica nesta aba e vai do seu navegador direto para o provedor. Isso serve para aprender, mas um app público deve chamar modelos a partir de um servidor, para que sua chave nunca seja enviada aos visitantes. Existem planos gratuitos, com limite de requisições.

## Experimente: Uma requisição e sua resposta

Execute a chamada e abra o AIMessage que volta. Conecte seu próprio modelo se quiser ver uma resposta ao vivo.

https://agenticademy.org/pt/paths/first-agents/02-one-model-call

## Verificação

O modelo é um modelo de chat que responde "Hello." e informa seu uso. O que isto imprime?

Código:

```
const reply = await model.invoke("Say hello.");
console.log(typeof reply.content, reply.usage_metadata.output_tokens > 0);
```

- **✓** string true
- object true
- string undefined
- Lança um erro: invoke precisa de um array de mensagens.

**Por quê:** invoke também aceita uma string simples, que o LangChain envolve em um HumanMessage. Para uma resposta em texto, content é uma string, e usage\_metadata conta os tokens de saída.

## O que você já sabe fazer

- Chamar um modelo de chat com invoke() e stream().
- Ler o content e o uso de tokens de um AIMessage.
- Apontar ChatOpenAI para um serviço compatível com OpenAI com baseURL.
- Explicar onde uma chave de API deve e não deve ficar.

---

[Voltar para Construa seus primeiros agentes](README.md) · [Próximo: Mensagens e estado](03-messages-and-state.md)
