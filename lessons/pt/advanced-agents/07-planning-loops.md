# Loops de planejamento

Agentes avançados · Capítulo 7 de 10 · 20 minutos

Plan-and-execute, ReWOO e LLMCompiler planejam o trabalho antes de fazê-lo, e diferem em chamadas ao modelo, supersteps e tempo.

## Objetivos

- Execute o plan-and-execute: um plano, um passo de cada vez e um replanejamento depois de cada passo.
- Execute o ReWOO: um plano com marcadores #E, as ferramentas e depois uma resposta.
- Execute o LLMCompiler: um grafo de tarefas cujas tarefas independentes rodam ao mesmo tempo.
- Passe na verificação.

## A leitura

### Planejar primeiro, depois agir

Os agentes vistos até aqui decidiam uma chamada de ferramenta de cada vez: chamar o modelo, rodar a ferramenta, chamar o modelo de novo. Esse é o loop ReAct. Um agente que planeja pede primeiro um plano ao modelo e depois o segue. Três padrões publicados fazem isso de jeitos diferentes. O lab os compara em três contadores: chamadas ao modelo, supersteps (uma rodada de execuções de nós, como no capítulo 1) e quanto tempo uma execução leva.

### Plan-and-execute

Um nó planner escreve uma lista de passos. Um nó agent faz o primeiro: o modelo escolhe uma chamada de ferramenta, e o nó a executa. Um nó replan lê o que já foi feito e depois responde ou reescreve os passos que faltam. O plano pode mudar depois de cada resultado, mas cada passo custa pelo menos duas chamadas ao modelo: uma para agir, outra para replanejar. A ideia vem do artigo Plan-and-Solve (2023) e do tutorial de plan-and-execute do LangGraph.

### ReWOO

O ReWOO (2023) escreve o plano inteiro em uma única chamada ao modelo e dá nome a cada resultado antes que ele exista: #E1 = search[…], depois #E3 = forecast[#E1]. #E1 é um marcador. Um nó tool roda os passos em ordem e coloca cada resultado no lugar do seu nome, sem chamar o modelo. Um nó solve chama o modelo mais uma vez para responder. São duas chamadas ao modelo no total, mas o plano não pode mudar depois de escrito.

### LLMCompiler

O LLMCompiler (2023) escreve o plano como um grafo de tarefas sem ciclos, um DAG: 3. forecast($1) quer dizer que a tarefa 3 precisa do resultado da tarefa 1. Um scheduler, a unidade de busca de tarefas (task fetching unit) do artigo, inicia cada tarefa assim que as tarefas de que ela precisa terminam, então tarefas que não precisam de nada umas das outras rodam ao mesmo tempo. Um nó join lê os resultados e depois responde ou pede um plano novo.

### Neste lab

Uma tarefa de pesquisa roda de três jeitos: a previsão do tempo de sábado onde larga cada uma de duas corridas de trilha, para a Halden, a loja fictícia. Cada grafo é LangGraph.js 1.5.1 de verdade, desenhado a partir de getGraphAsync(). No Node, você também poderia escrever o scheduler com a API funcional, como um entrypoint cujas chamadas a task() rodam em paralelo. task() precisa de AsyncLocalStorage, que a versão para navegador não tem, então aqui o nó schedule inicia promises simples e passa config à mão. O modelo é roteirizado, e cada chamada leva um tempo roteirizado, acelerado na tela e contado como tempo simulado.

## Experimente: Uma tarefa, três planejadores

Rode a mesma tarefa de pesquisa como plan-and-execute, ReWOO e LLMCompiler e depois compare as chamadas ao modelo, os supersteps e o tempo simulado de cada um.

https://agenticademy.org/pt/paths/advanced-agents/07-planning-loops

## Verificação

### Pergunta 1 de 3

Uma tarefa precisa de cinco buscas que não dependem umas das outras e depois de uma resposta. Qual padrão pode terminar mais cedo?

- Plan-and-execute: replan mantém o plano atualizado.
- ReWOO: ele chama o modelo só duas vezes.
- **✓** LLMCompiler: as cinco buscas podem rodar ao mesmo tempo.
- Os três levam o mesmo tempo, porque as buscas são as mesmas.

**Por quê:** O scheduler do LLMCompiler inicia cada tarefa cujas entradas estão prontas, então as cinco buscas se sobrepõem e levam mais ou menos o tempo da mais lenta. O ReWOO também chama o modelo só duas vezes, mas o nó tool dele roda uma busca por superstep, uma depois da outra. O plan-and-execute acrescenta duas chamadas ao modelo por busca. As buscas são as mesmas; quando elas rodam, não.

### Pergunta 2 de 3

Um plano do ReWOO tem o passo #E3 = forecast[#E1]. O que acontece com #E1 antes de forecast rodar?

- **✓** O nó tool coloca o resultado do passo #E1 no lugar dele.
- O modelo é chamado de novo para transformar #E1 em uma cidade.
- forecast recebe o texto “#E1” e o procura.
- O nó solve o preenche no final.

**Por quê:** #E1 é um marcador para o resultado do passo chamado #E1. O nó tool o substitui por esse resultado antes de chamar forecast, então nenhuma chamada ao modelo é necessária entre os passos. É assim que o ReWOO se vira com duas chamadas ao modelo: uma para planejar e outra no nó solve, que roda depois de todas as ferramentas, tarde demais para preencher qualquer coisa.

### Pergunta 3 de 3

A Lake Loop foi cancelada, então a busca pela cidade de largada dela retorna “Nenhum resultado.” Qual padrão pode tirar a previsão da Lake Loop antes que ela rode?

- **✓** Plan-and-execute: replan lê cada resultado antes do próximo passo.
- ReWOO: o nó tool pula um passo cuja entrada não encontrou nada.
- LLMCompiler: o scheduler pula uma tarefa cuja entrada não encontrou nada.
- Nenhum: um plano não pode mudar depois de escrito.

**Por quê:** O plan-and-execute chama replan depois de cada passo, então pode tirar a previsão dos passos que faltam: é para isso que servem as chamadas ao modelo a mais que ele paga. O nó tool do ReWOO só preenche resultados, então roda forecast[#E2] com “Nenhum resultado.” como cidade. O scheduler do LLMCompiler também roda forecast($2) como planejado; só o nó join, depois de todas as tarefas, pode pedir um plano novo.

## O que você já sabe fazer

- Construir plan-and-execute, ReWOO e LLMCompiler como grafos do LangGraph.js.
- Contar quanto cada padrão custa em chamadas ao modelo, supersteps e tempo, e dizer de onde vem a diferença.
- Escolher um padrão: replanejar quando os resultados podem mudar o plano, planejar uma vez quando não podem, e rodar ao mesmo tempo os passos independentes.

---

[Voltar para Agentes avançados](README.md) · [Próximo: Gastar computação](08-spending-compute.md)
