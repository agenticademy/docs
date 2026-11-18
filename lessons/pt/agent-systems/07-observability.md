# Observabilidade: traces e execuções

Sistemas de agentes e padrões · Capítulo 7 de 8 · 20 minutos

Registrar cada chamada de modelo, chamada de ferramenta e passo como uma árvore de execuções, e lê-la para achar o que está lento ou quebrado.

## Objetivos

- Encontre a execução de modelo ou de ferramenta mais lenta no trace.
- Encontre a execução de ferramenta que falhou e a execução pai dela.
- Exporte as execuções no formato do LangSmith.
- Passe na verificação.

## A leitura

### Execuções e traces

A resposta de um agente esconde muitos passos. O tracing registra cada um como uma execução (run): um grafo, um nó, uma chamada de modelo ou uma chamada de ferramenta, com suas entradas, saídas, horários de início e fim, e qualquer erro. As execuções por trás de uma requisição formam uma árvore, chamada de trace.

O LangChain.js e o LangGraph.js fazem tracing por meio de callbacks. Passe um tracer como o LangChainTracer nos callbacks da config, e cada runnable informa a ele seu início e seu fim.

### O formato de execução do LangSmith

Cada execução tem um run\_type, como chain, llm ou tool, um id, um trace\_id compartilhado pela árvore inteira e um parent\_run\_id. Seu dotted\_order é uma chave formada pelo horário de início e pelo id de cada execução desde a raiz até ela, então ordenar por ela organiza a árvore na ordem em que as coisas aconteceram. As execuções de modelo trazem usage\_metadata com a contagem de tokens. O LangGraph acrescenta thread\_id, langgraph\_node e langgraph\_step aos metadados.

### Supersteps e a árvore de execuções

O LangGraph executa um grafo em supersteps. Em cada superstep, os nós que estão na vez rodam, e suas atualizações são aplicadas juntas antes de o próximo superstep começar. langgraph\_step é o número desse superstep, então um trace pode ser lido de duas formas: como uma árvore de execuções, cada execução sob a sua execução pai, e como uma linha do tempo de supersteps, um grupo de execuções por passo. A cascata do laboratório agrupa suas linhas por superstep.

### O que procurar

Latência: qual execução é longa, e se é o modelo ou uma ferramenta. Erros: uma execução de ferramenta que falhou e o nó que a chamou. Tokens: quais chamadas leem mais. Loops: o mesmo passo de novo e de novo.

Os traces guardam prompts, resultados de ferramentas e dados de clientes, então trate-os como sensíveis. O SDK do LangSmith pode rodar um anonimizador em cada execução antes de enviá-la. O OpenTelemetry é o padrão geral para traces entre serviços; o LangSmith também aceita traces OpenTelemetry.

## Experimente: A cascata de execuções

Cinco passos: execute o agente e veja o trace dele crescer, leia os supersteps, encontre a execução mais lenta e a ferramenta que falhou, depois exporte as execuções.

https://agenticademy.org/pt/paths/agent-systems/07-observability

## Verificação

### Pergunta 1 de 5

O que o dotted\_order oferece?

- O custo de uma execução.
- **✓** Uma chave ordenável que coloca cada execução na árvore do seu trace, em ordem de tempo.
- O nome do modelo.

**Por quê:** Ela é formada pelo horário de início e pelo id de cada execução desde a raiz até esta, então ordenar as execuções por ela reconstrói a árvore na ordem em que as coisas aconteceram, sem seguir os vínculos com o pai um por um.

### Pergunta 2 de 5

Duas execuções em um trace têm o mesmo langgraph\_step. O que isso diz a você?

- Elas foram chamadas pela mesma ferramenta.
- **✓** Elas rodaram no mesmo superstep do grafo.
- Elas levaram o mesmo tempo.

**Por quê:** langgraph\_step é o número do superstep. Em cada superstep, o LangGraph executa os nós que estão na vez e aplica as atualizações deles juntas; cada execução dentro desses nós carrega esse número.

### Pergunta 3 de 5

O que é um trace?

- Um arquivo de log de erros.
- **✓** A árvore de execuções, de modelo, ferramenta e chain, por trás de uma requisição.
- Uma captura de tela do app.

**Por quê:** Uma requisição vira um trace; cada passo dentro dele é uma execução com sua execução pai.

### Pergunta 4 de 5

Uma execução de ferramenta falhou, mas o agente respondeu mesmo assim. Por que a execução pai não está marcada como falha?

- O tracer esconde erros.
- **✓** O nó de ferramentas capturou o erro e o devolveu ao modelo como resultado, então o próprio nó foi concluído.
- Execuções pai nunca falham.

**Por quê:** Um erro é registrado na execução que o lançou. Aqui o nó o transformou em uma ToolMessage de erro, o modelo a leu e tentou de novo.

### Pergunta 5 de 5

O que você deve evitar enviar a um serviço de tracing compartilhado?

- Os tempos das execuções.
- **✓** Dados pessoais de clientes dentro de prompts e resultados de ferramentas, a menos que você precise deles e tenha permissão.
- Os tipos de execução.

**Por quê:** Os traces registram entradas e saídas por completo. Oculte ou limite o que é capturado antes que saia do seu sistema, por exemplo com o anonimizador do SDK.

## O que você já sabe fazer

- Ler uma linha do tempo de supersteps e uma árvore de execuções.
- Encontrar a execução lenta ou que falhou em um trace, e a execução que a chamou.
- Nomear os principais campos do formato de execução do LangSmith.
- Ocultar dados pessoais antes que um trace saia do seu sistema.

---

[Voltar para Sistemas de agentes e padrões](README.md) · [Próximo: Avaliação e segurança](08-evaluation-and-safety.md)
