# Engenharia de contexto e recuperação

Sistemas de agentes e padrões · Capítulo 6 de 8 · 25 minutos

Escolher o que cada chamada de modelo lê: grep ou busca vetorial, descarregar resultados grandes, um primeiro olhar sobre subagentes, e uma memória que dura entre threads.

## Objetivos

- Rode o grep e a busca vetorial com as mesmas palavras e compare.
- Deixe um resultado de ferramenta grande ser descarregado em vez de encher o contexto.
- Passe uma busca a subagentes e compare os tamanhos de contexto.
- Passe na verificação.

## A leitura

### O que cada chamada lê

Uma chamada de modelo lê seu contexto: o system prompt, as mensagens até ali e cada resultado de ferramenta. Engenharia de contexto é decidir o que entra. Pouco demais e o agente chuta; demais e ele custa mais, fica mais lento e se distrai com texto que não importa, ou que foi plantado.

### grep ou vetores

O grep encontra texto exato, linha a linha: um número de pedido, um id de chamado, uma data. Ele não custa nada e perde tudo o que estiver escrito de outro jeito.

A busca vetorial encontra significado parecido. Cada trecho de cada arquivo vira um vetor (aqui, 384 números do modelo de embedding all-MiniLM-L6-v2), e os trechos são ordenados pela similaridade de cosseno com o vetor da consulta. Ela encontra “payment taken two times” para “charged twice”, mas para 48213 coloca chamados sobre os pedidos 48144 e 48231 à frente do certo. Filtros como uma pasta ou uma data deixam de fora trechos desatualizados ou sem relação.

### Descarregar, skills e subagentes

Uma ferramenta pode devolver muito mais do que um contexto comporta. A remoção salva um resultado grande em um arquivo e dá ao modelo o caminho e uma prévia curta; o agente então faz grep no arquivo e lê só as linhas de que precisa.

As skills seguem a mesma ideia: o agente vê o nome de cada skill e uma descrição de uma linha, e só lê as instruções completas (um arquivo SKILL.md) quando uma tarefa precisa delas.

Um subagente recebe um contexto novo só com sua tarefa e devolve um relatório, então a leitura dele fica fora do contexto principal. Agentes avançados, capítulos 9 e 10, trata a fundo de subagentes, limites e compactação.

### Memória entre threads

Uma thread é uma conversa: o checkpointer dela guarda as mensagens daquela thread, e uma thread nova começa vazia. A memória de longo prazo é um store que todas as threads compartilham. O InMemoryStore do LangGraph guarda itens sob um namespace, como customers / mara-lind; uma thread coloca uma nota ali, e uma thread posterior a busca. Em produção, o store é um banco de dados.

### Neste lab

O agente de suporte da Halden trabalha no chamado T-48213. O harness dele recria o Deep Agents (um harness open source da LangChain) no LangGraph.js: ferramentas de sistema de arquivos, remoção acima de 20.000 tokens e subagentes de tarefa. O passo de memória usa o próprio InMemoryStore do LangGraph.js.

## Experimente: O que o agente lê

Busque as mesmas palavras com grep e com vetores, deixe a exportação de pagamentos ser descarregada, passe a leitura para subagentes e depois salve uma nota que uma thread posterior lê de volta.

https://agenticademy.org/pt/paths/agent-systems/06-context-engineering

## Verificação

### Pergunta 1 de 4

O agente precisa encontrar todos os chamados que mencionam o pedido 48213. Qual busca serve melhor?

- Busca vetorial.
- **✓** grep por 48213.
- Pedir ao modelo que se lembre.

**Por quê:** Um id exato é para isso que serve o grep. A busca vetorial ordena por significado, e neste lab ela coloca chamados sobre os pedidos 48144 e 48231 à frente do certo.

### Pergunta 2 de 4

Uma ferramenta devolve uma exportação de 40.000 linhas. O que a remoção coloca no contexto?

- Todas as 40.000 linhas.
- **✓** O caminho do arquivo e uma prévia curta; o agente lê páginas dele quando precisa.
- Nada.

**Por quê:** O resultado completo é salvo em um arquivo, então o contexto continua pequeno e o agente ainda pode fazer grep e paginar os dados.

### Pergunta 3 de 4

O que volta de um subagente para o agente que o iniciou?

- A conversa inteira dele.
- **✓** Um relatório final do seu trabalho.
- As ferramentas dele.

**Por quê:** A leitura do subagente fica no contexto dele. Só o relatório entra no contexto do agente pai, o que mantém o pai pequeno e o texto plantado fora dele.

### Pergunta 4 de 4

A thread 1 salvou uma nota no store. A thread 2 começa sem mensagens. Ela consegue ler a nota?

- Não: uma thread nova começa vazia.
- **✓** Sim: todas as threads compartilham o store; só as mensagens pertencem a uma thread.
- Só se a thread 2 copiar as mensagens da thread 1.

**Por quê:** O checkpointer mantém separadas as mensagens de cada thread. O store fica fora das threads, então qualquer thread pode buscar no namespace da cliente.

## O que você já sabe fazer

- Escolher entre grep e busca vetorial para uma consulta, e dizer quando um filtro de pasta ou de data ajuda.
- Descarregar resultados de ferramenta grandes e paginá-los.
- Usar subagentes para manter o contexto de um agente principal pequeno e limpo.
- Salvar uma nota em um store de longo prazo e lê-la de volta em uma thread posterior.

---

[Voltar para Sistemas de agentes e padrões](README.md) · [Próximo: Observabilidade: traces e execuções](07-observability.md)
