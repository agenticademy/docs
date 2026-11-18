# Viagem no tempo

Agentes avançados · Capítulo 4 de 10 · 20 minutos

Cada superstep salva um checkpoint, então você pode ler o histórico de uma execução, repetir a partir de um passo passado, editar um estado passado e bifurcar um segundo final.

## Objetivos

- Execute um grafo de cinco nós em um thread e leia o histórico de checkpoints dele.
- Volte ao passo 3 e repita a partir dele.
- Edite o estado no passo 3, bifurque e compare os dois finais.
- Coloque os passos de uma bifurcação em ordem.

## A leitura

### Um thread é uma linha do tempo

Compile um grafo com um checkpointer, execute-o com um thread\_id, e o LangGraph salva um checkpoint do estado inteiro depois de cada superstep, como na trilha Sistemas de agentes. Cada checkpoint tem o próprio checkpoint\_id, um step e um source nos metadados (o step -1 é a entrada), e um parentConfig que aponta o checkpoint anterior. Ligados pelos pais, os checkpoints de um thread formam uma linha do tempo, e uma linha do tempo pode se ramificar.

### Ler o passado

getStateHistory({ configurable: { thread\_id } }) lista todos os checkpoints do thread, do mais novo para o mais antigo. Cada item é um snapshot do estado: values, next (os nós que devem rodar), config e metadata. getState com um checkpoint\_id na config lê um deles. Isso é voltar no tempo: você olha um estado passado e não muda nada.

### Repetir e bifurcar

invoke(null, config), com uma config que aponta um checkpoint passado, repete a partir dele. Os nós antes desse checkpoint não rodam de novo; os depois dele rodam, então um modelo real pode responder diferente na segunda vez. Os novos checkpoints crescem em um novo ramo que começa com uma cópia cujo source é fork. Os antigos continuam lá.

updateState(config, values, asNode) muda o passado sem apagá-lo. Ele escreve um novo checkpoint, com source update, em cima daquele que config aponta, como se o nó asNode tivesse retornado values, e retorna a config do novo checkpoint. Rode invoke(null, thatConfig) e o thread se bifurca: o primeiro final continua no histórico e um segundo cresce a partir da sua edição. getState sem checkpoint\_id lê o checkpoint mais novo, que agora é o fim da bifurcação.

### Durabilidade

A opção durability de uma execução decide quando os checkpoints são escritos. "async", o padrão, salva cada um enquanto o próximo superstep roda; "sync" salva antes de o próximo superstep começar; "exit" salva só quando a execução termina. "exit" escreve menos, mas não deixa nenhum passo passado para onde voltar, nem nada de onde retomar se o processo cair no meio.

### Neste lab

A verificação de reembolso da Halden para o ticket T-48213 executa cinco nós em sequência: read\_ticket, classify, list\_charges, decide e draft\_reply. O LangGraph.js 1.5.1 e o MemorySaver dele rodam nesta página, e classify, decide e draft\_reply consultam um modelo roteirizado. A versão para navegador do LangGraph.js não tem AsyncLocalStorage, então cada nó passa a config dele para o modelo de forma explícita.

## Experimente: Um trilho de checkpoints

Três passos: execute o grafo e leia os checkpoints dele, volte ao passo 3 e repita, depois edite o passo 3, bifurque e compare os dois finais.

https://agenticademy.org/pt/paths/advanced-agents/04-time-travel

## Verificação

Coloque em ordem os passos para bifurcar uma execução a partir de um checkpoint passado.

1. Execute o grafo com um checkpointer e um thread\_id.
2. Liste os checkpoints do thread com getStateHistory.
3. Escolha pelo checkpoint\_id o checkpoint para onde voltar.
4. Chame updateState com a config desse checkpoint e os novos valores.
5. Chame invoke(null, config) com a config que updateState retornou.
6. Compare o final da bifurcação com o primeiro, que continua no histórico.

**Por quê:** Os checkpoints só existem depois que uma execução em um thread os salvou, e você encontra um checkpoint passado em getStateHistory pelo checkpoint\_id dele. updateState precisa da config desse checkpoint e retorna a config do novo checkpoint que escreve; invoke(null, …) com essa config roda a bifurcação. Nada é sobrescrito, então o primeiro final continua lá para comparar.

## O que você já sabe fazer

- Ler os checkpoints de um thread com getStateHistory, e um deles com getState e o checkpoint\_id dele.
- Repetir uma execução a partir de um checkpoint passado, e dizer quais nós rodam de novo.
- Editar um estado passado com updateState, bifurcar o thread e comparar os dois finais.
- Escolher um modo de durabilidade sabendo para onde cada um deixa você voltar.

---

[Voltar para Agentes avançados](README.md) · [Próximo: Loops que resistem](05-loops-that-survive.md)
