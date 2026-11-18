# Guardrails

Construa seus primeiros agentes · Capítulo 8 de 8 · 22 minutos

Verificações em código ao redor do modelo: verificações de entrada e de saída, listas de ferramentas permitidas e limites de passos, em camadas.

## Objetivos

- Ative cada guarda e veja o que ela impede.
- Pare os quatro ataques em uma só repetição.
- Preveja o que o limite de passos faz.

## A leitura

### Regras das quais o modelo não consegue escapar na conversa

Um guardrail é uma verificação no seu código, fora do modelo. Um prompt pode pedir ao modelo que siga uma regra, e uma mensagem esperta pode convencê-lo a abandonar essa regra. Código que recusa uma chamada não pode ser convencido de nada.

### Quatro tipos de verificação

Verificações de entrada testam a mensagem do usuário antes que o modelo a leia, e a recusam ou a marcam. Verificações de saída testam a resposta antes que alguém a veja: por exemplo, todo valor que ela cita deve aparecer em um resultado de ferramenta, para que o modelo não possa inventar um.

Listas de permissões limitam o que pode acontecer: quais ferramentas podem rodar, quais hosts elas podem acessar, quais valores podem movimentar. Elas negam por padrão, então uma ferramenta ou um argumento que ninguém listou é recusado, mesmo quando uma instrução escondida em um chamado o pede. Coloque a lista de permissões ao redor do nó de ferramentas, para que ela recuse antes que a ferramenta rode.

Limites de passos param loops descontrolados. recursionLimit, na config da execução, limita os supersteps, e o LangGraph lança GraphRecursionError quando ele é atingido. Capture-o por err.name e transforme-o em uma parada clara. Um orçamento de tokens ou de chamadas ao modelo funciona do mesmo jeito.

### Em camadas

Nenhuma verificação sozinha pega tudo. A verificação de entrada nunca vê texto que chega em um resultado de ferramenta; a lista de permissões não lê respostas. Então você as empilha, e quando uma camada falha, a próxima ainda pode parar o ataque.

Modelos de segurança dedicados são outra camada: classificadores como os modelos de content safety Nemotron da NVIDIA e o Llama Nemotron safety guard leem uma mensagem e a rotulam como segura ou insegura. Eles pegam o que um padrão deixa passar, mas também são modelos, então mantenha também as verificações em código.

## Experimente: Quatro guardas, quatro ataques

Dispare quatro ataques contra um agente sem guardas. Depois ative uma guarda por ataque e veja o que ela para. Termine com uma repetição que pare os quatro.

https://agenticademy.org/pt/paths/first-agents/08-guardrails

## Verificação

O modelo roteirizado sempre pede mais uma chamada de ferramenta. O que acontece?

Código:

```
await agent.invoke(
  { messages: [new HumanMessage("Check ticket T-48213")] },
  { recursionLimit: 6 },
);
```

- Roda para sempre.
- **✓** Lança GraphRecursionError depois de 6 passos.
- Devolve a última mensagem depois de 6 passos, sem erro.
- recursionLimit só se aplica a subgrafos.

**Por quê:** recursionLimit limita o número de supersteps. Quando ele é atingido, o LangGraph lança GraphRecursionError, que seu código captura e transforma em uma parada clara.

## O que você já sabe fazer

- Escrever verificações de entrada e de saída ao redor de um modelo.
- Envolver um nó de ferramentas com uma lista de permissões que nega por padrão.
- Limitar os passos de um agente e tratar GraphRecursionError.
- Explicar por que os guardrails vêm em camadas.

---

[Voltar para Construa seus primeiros agentes](README.md)
