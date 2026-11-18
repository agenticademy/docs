# Um harness de deep agent

Agentes avançados · Capítulo 9 de 10 · 20 minutos

Um deep agent planeja com uma lista de afazeres, guarda notas em arquivos virtuais, passa partes de um trabalho a subagentes com um contexto novo e pausa antes das ferramentas que você nomear.

## Objetivos

- Inicie o trabalho e veja o agente escrever e atualizar sua lista de afazeres.
- Aprove a gravação das notas e veja a árvore de arquivos crescer.
- Rode três subagentes em paralelo e compare os tamanhos de contexto deles.
- Passe na verificação.

## A leitura

### Um harness para trabalhos longos

Um harness é o código em volta do loop do modelo: o prompt, as ferramentas e o que cada chamada vê. O pacote de código aberto deepagents, da LangChain, é um deles, construído sobre o LangGraph. O anúncio dele em 2025 citou quatro partes: um prompt detalhado, uma ferramenta de planejamento, subagentes e um sistema de arquivos.

### Afazeres e arquivos

write\_todos({ todos }) recebe a lista inteira a cada vez, cada item pending, in\_progress ou completed, e a guarda em state.todos. Não faz mais nada: o ganho é que o modelo relê seu plano a cada chamada. Duas chamadas no mesmo turno são recusadas. Para ativá-lo, adicione o todoListMiddleware() da LangChain a middleware.

As ferramentas de arquivos (ls, read\_file, write\_file, edit\_file, glob, grep) trabalham sobre state.files, um sistema de arquivos virtual no estado do grafo. Os subagentes podem ler as notas ali, e elas sobrevivem a um resumo das mensagens antigas.

### Subagentes

task({ description, subagent\_type }) inicia um subagente com um contexto novo: seu próprio system prompt e a descrição, nenhuma das mensagens de quem o chamou. Ele começa com os arquivos de quem o chamou, os arquivos que ele grava são mesclados de volta, e só sua última mensagem retorna, como resultado da ferramenta. Chamadas no mesmo turno rodam em paralelo. Um subagente de uso geral, com as ferramentas de quem o chama, já vem por padrão.

### Skills, memória e aprovações

Uma skill é uma pasta com um arquivo SKILL.md. Com skills: ['/skills/'], o prompt lista o nome e a descrição de cada skill, e o agente só lê o arquivo quando uma tarefa precisa dele. A opção memory carrega arquivos AGENTS.md no prompt. interruptOn, com um checkpointer, nomeia ferramentas que pausam a execução até que uma pessoa aprove, edite ou rejeite cada chamada. Esta página pausa em um breakpoint (interruptBefore), porque interrupt() precisa de AsyncLocalStorage.

### Menos andaime

Cada parte custa tokens em toda chamada. O deepagents 1.12 (2026-07-29) tornou a lista de afazeres opcional e deixou vazio o system prompt genérico: os modelos atuais precisam de menos orientação. O texto da Anthropic sobre design de harness (2026-03-24) concorda: a cada modelo novo, remova as partes de que ele não precisa mais.

### Neste laboratório

Um harness recriado escreve um relatório sobre as reclamações de reembolso na Halden, uma loja fictícia. Ao contrário do deepagents 1.16, ele ativa a lista com todos: true, mostra as skills ao pesquisador sem uma lista de skills própria, mantém a antiga linha genérica do prompt e não tem memória AGENTS.md. O agente principal pausa antes de write\_file e task para que você possa acompanhá-lo; no seu próprio agente, pause antes das ferramentas que mudam coisas fora dele.

## Experimente: Uma pesquisa, uma parte de cada vez

Inicie o trabalho e acompanhe a lista de afazeres, deixe o agente principal gravar suas notas, depois aprove três subagentes e compare os tamanhos de contexto deles.

https://agenticademy.org/pt/paths/advanced-agents/09-deep-agent-harness

## Verificação

O contexto do agente principal tem cerca de 1.400 tokens quando o modelo dele faz esta chamada. O pesquisador lê cerca de 600 tokens de arquivos, edita a nota e termina com um relato de 40 tokens. O que muda no estado do agente principal?

Código:

```
const lead = createDeepAgent({
  systemPrompt: 'You write briefs.',
  subagents: [{
    name: 'researcher',
    description: 'Researches a cause.',
    systemPrompt: 'Edit the note, then report.',
  }],
});

// The lead's model asks for:
task({
  subagent_type: 'researcher',
  description:
    'Late refunds: /notes/late-refund.md',
})
```

- As mensagens dele recebem toda a conversa do pesquisador, cerca de 650 tokens; os arquivos dele não mudam.
- As mensagens dele recebem um resultado de ferramenta, o relato de 40 tokens; os arquivos dele não mudam, porque o pesquisador editou uma cópia.
- **✓** As mensagens dele recebem um resultado de ferramenta, o relato de 40 tokens, e a nota editada é mesclada aos arquivos dele.
- Nada muda até o agente principal ler /notes/late-refund.md por conta própria.

**Por quê:** task inicia o pesquisador com um contexto novo: seu próprio system prompt e a descrição, nenhuma das mensagens do agente principal. O que ele lê fica no próprio contexto. Só a última mensagem dele volta, como resultado da ferramenta task, então o agente principal cresce cerca de 40 tokens, e os arquivos que o pesquisador gravou são mesclados ao state.files do agente principal. No laboratório, três pesquisadores leram milhares de tokens enquanto o agente principal cresceu três relatos curtos.

## O que você já sabe fazer

- Ativar a lista de afazeres de um deep agent adicionando todoListMiddleware() ao middleware dele, e ler o plano em state.todos.
- Guardar notas e rascunhos em state.files com ls, read\_file, write\_file, edit\_file, glob e grep.
- Passar trabalho autocontido a subagentes via task, e usar os relatos e arquivos que voltam.
- Pausar um deep agent antes de ferramentas escolhidas com interruptOn e um checkpointer, e remover o andaime de que um modelo mais novo não precisa mais.

---

[Voltar para Agentes avançados](README.md) · [Próximo: Agentes de longo horizonte](10-long-horizon.md)
