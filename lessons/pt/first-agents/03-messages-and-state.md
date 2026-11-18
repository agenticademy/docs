# Mensagens e estado

Construa seus primeiros agentes · Capítulo 3 de 8 · 18 minutos

Os quatro tipos de mensagem, por que a conversa inteira é enviada toda vez e como o LangGraph mescla o estado com reducers.

## Objetivos

- Mude a mensagem de sistema e compare as respostas.
- Acrescente uma mensagem e substitua outra pelo seu id.
- Preveja o que o reducer devolve.

## A leitura

### Quatro tipos de mensagem

Uma conversa é uma lista de mensagens, e cada mensagem tem um tipo. SystemMessage guarda suas instruções para o modelo. HumanMessage é o que o usuário escreveu. AIMessage é a resposta do modelo; ela pode trazer tool\_calls, pedidos para executar ferramentas. ToolMessage leva de volta o resultado de uma ferramenta, ligado ao seu pedido por tool\_call\_id. Os quatro vêm de @langchain/core/messages.

### O modelo não lembra de nada

Um modelo de chat não guarda memória entre chamadas. Cada chamada envia a lista inteira, e a resposta depende só dessa lista. Para continuar uma conversa, você acrescenta a resposta e a próxima pergunta, e envia tudo de novo. É também por isso que a contagem de tokens de entrada cresce a cada turno.

### Estado e reducers

No LangGraph, os dados que um grafo leva de passo em passo são o seu estado. Um nó não sobrescreve o estado: ele devolve uma atualização, e um reducer decide como a atualização se mescla ao que já está lá. Sem um reducer, o valor novo substitui o antigo.

Para mensagens, o LangGraph oferece MessagesAnnotation, um estado com uma chave, messages, mesclada por messagesStateReducer. Toda mensagem recebe um id; o reducer atribui um se ela não tiver. Uma mensagem com um id novo é acrescentada. Uma mensagem com um id que já está na lista substitui aquela mensagem no lugar. Um RemoveMessage com um id apaga aquela mensagem. É assim que um grafo edita uma conversa, por exemplo para cortar turnos antigos, sem reconstruir a lista.

## Experimente: A pilha de mensagens

Chame o modelo, mude a mensagem de sistema e chame de novo, depois envie atualizações de estado e veja o reducer acrescentar, substituir e remover mensagens.

https://agenticademy.org/pt/paths/first-agents/03-messages-and-state

## Verificação

O que messagesStateReducer devolve?

Código:

```
messagesStateReducer(
  [new HumanMessage({ id: "a", content: "Hi" }), new AIMessage({ id: "b", content: "Hello" })],
  [new AIMessage({ id: "b", content: "Hello!" }), new HumanMessage({ id: "c", content: "Thanks" })],
);
```

- Quatro mensagens: a, b, b, c.
- **✓** Três mensagens: a, b (agora "Hello!"), c.
- Duas mensagens: b e c.
- Lança um erro porque b aparece duas vezes.

**Por quê:** A atualização com id b substitui o b existente no lugar, e c é novo, então é acrescentado.

## O que você já sabe fazer

- Usar corretamente os quatro tipos de mensagem.
- Explicar por que a conversa é enviada a cada chamada.
- Prever como o reducer de mensagens do LangGraph mescla uma atualização.

---

[Voltar para Construa seus primeiros agentes](README.md) · [Próximo: Seu primeiro grafo](04-your-first-graph.md)
