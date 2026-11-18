# Agentes avançados

Avançado · Para desenvolvedores · 10 capítulos · 205 min

Para desenvolvedores que já sabem criar sistemas de agentes. Você trabalha com grafos grandes e seu estado, roteia com Command e Send, volta e ramifica execuções, cria loops que se recuperam de falhas e sabem quando parar, compara padrões de planejamento e roda um harness de deep agents numa tarefa longa. Cada laboratório roda o LangGraph.js 1.5.1 real no seu navegador, com um modelo roteirizado. O harness de deep agents é recriado, e as telas dizem isso.

## Antes de começar

A trilha Sistemas de agentes e padrões, ou o mesmo conhecimento: grafos, ferramentas, o loop do agente, checkpoints e traces.

## No fim você sabe

- Projetar o estado de um grafo com reducers e Overwrite, e prever como escritas em paralelo se combinam.
- Rotear com Command e Send, e montar equipes a partir de subgrafos.
- Voltar, editar e ramificar uma execução a partir dos seus checkpoints.
- Criar loops que tentam de novo, compensam, retomam após uma queda e param por limites, orçamentos ou decisão de uma pessoa.
- Escolher um padrão de planejamento ou de gasto de computação, e rodar um harness de deep agents numa tarefa longa.
- Levar uma tarefa longa por várias sessões com arquivos de passagem, compactação e um Store, e avaliar a trajetória com regras em código.

## Capítulos

1. [Estado em escala](01-state-at-scale.md): Como o LangGraph.js 1.5.1 combina as escritas em uma chave do estado: campos de StateSchema com e sem reducer, a ordem das escritas em paralelo, Overwrite e UntrackedValue.
2. [Command e Send](02-command-and-send.md): Um nó pode atualizar o estado e escolher o próximo nó em um só passo com Command, e distribuir o trabalho entre workers em paralelo com Send, quantos o modelo pedir.
3. [Subgrafos e equipes](03-subgraphs-and-teams.md): Monte equipes de agentes com subgrafos, decida como elas compartilham o estado, leia seus namespaces no stream e compare padrões multiagente por chamadas ao modelo e tokens.
4. [Viagem no tempo](04-time-travel.md): Cada superstep salva um checkpoint, então você pode ler o histórico de uma execução, repetir a partir de um passo passado, editar um estado passado e bifurcar um segundo final.
5. [Loops que resistem](05-loops-that-survive.md): Políticas de nova tentativa, timeouts, handlers de erro e checkpoints mantêm uma execução longa de pé quando uma ferramenta falha ou o processo para.
6. [Loops que param](06-loops-that-stop.md): Um loop precisa de paradas que valem não importa o que o modelo faça: um limite de recursão, um orçamento de chamadas, um detector de chamadas repetidas e uma pessoa que pode dizer não.
7. [Loops de planejamento](07-planning-loops.md): Plan-and-execute, ReWOO e LLMCompiler planejam o trabalho antes de fazê-lo, e diferem em chamadas ao modelo, supersteps e tempo.
8. [Gastar computação](08-spending-compute.md): Loops avaliador-otimizador, reflexão, best-of-n e busca em árvore gastam mais chamadas ao modelo para chegar a uma resposta melhor, e cada um precisa de um orçamento.
9. [Um harness de deep agent](09-deep-agent-harness.md): Um deep agent planeja com uma lista de afazeres, guarda notas em arquivos virtuais, passa partes de um trabalho a subagentes com um contexto novo e pausa antes das ferramentas que você nomear.
10. [Agentes de longo horizonte](10-long-horizon.md): Um trabalho que se estende por várias sessões precisa de compactação dentro de uma sessão, arquivos de passagem e memória entre sessões, e uma verificação de toda a sua trajetória.

https://agenticademy.org/pt/paths/advanced-agents
