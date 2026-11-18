# Subgrafos e equipes

Agentes avançados · Capítulo 3 de 10 · 20 minutos

Monte equipes de agentes com subgrafos, decida como elas compartilham o estado, leia seus namespaces no stream e compare padrões multiagente por chamadas ao modelo e tokens.

## Objetivos

- Execute uma equipe de equipes e leia os namespaces do seu stream.
- Pause dentro de uma equipe e abra o estado dela.
- Compare subagentes com repasses em chamadas ao modelo e tokens.
- Preveja o que um subgrafo emite no stream.

## A leitura

### Subgrafos

Um grafo compilado pode ser um nó de outro grafo: um subgrafo. Quando os dois compartilham uma chave de estado, o subgrafo compilado é o nó, como em addNode("research\_team", researchTeam). Ele lê as chaves que compartilha com o pai, o que ele escreve nelas volta, e as chaves que só ele tem ficam lá dentro.

Sem nenhuma chave compartilhada, chame o subgrafo a partir de um nó envoltório que converte o estado do pai na entrada do subgrafo, e a saída dele de volta em uma atualização. Passe { subgraphs: [team] } para addNode para que getGraphAsync e getState continuem encontrando o subgrafo.

### Namespaces e streaming

Cada execução de um subgrafo salva seus checkpoints em um namespace próprio dentro da thread do pai, nomeado pelo seu nó e pela sua tarefa: research\_team:<task id>. stream(input, { streamMode: "updates", subgraphs: true }) produz pares [namespace, update], com [] para o grafo de cima. getGraphAsync({ xray: true }) desenha os nós próprios dos subgrafos, como research\_team:lead.

Enquanto uma execução está pausada dentro de um subgrafo, getState(config, { subgraphs: true }) também devolve o estado próprio do subgrafo. Em um navegador, interrupt() não pode rodar, porque precisa de AsyncLocalStorage, então este laboratório pausa com interruptBefore e passa config para cada subgrafo à mão.

### Equipes de equipes e repasses

Um supervisor pode comandar outros supervisores. No laboratório, um chefe de suporte roteia com Command({ goto }) para duas equipes, e o líder de cada equipe distribui partes do trabalho entre seus workers. Um nó dentro de um subgrafo também pode passar o controle para um nó do pai: new Command({ graph: Command.PARENT, goto: "writer", update }). Isso é um repasse: o próximo agente assume com o histórico que você colocou em update.

### Escolher um padrão

A documentação de JS do LangChain nomeia cinco padrões multiagente. Subagentes: um agente principal chama outros agentes como ferramentas e recebe cada resultado de volta. Repasses: o agente no comando muda. Skills: um agente carrega prompts de especialista quando precisa deles. Router: uma etapa classifica a entrada e a envia para agentes. Fluxo personalizado: o seu próprio grafo do LangGraph. O capítulo 5 de Sistemas de agentes chamou de repasses as suas chamadas transfer\_to\_<agent>, mas o supervisor dele usava subagentes: cada resultado voltava para ele.

A documentação os compara (lida em 2026-11-04): uma solicitação de uma só vez leva 4 chamadas ao modelo com subagentes e 3 com repasses; uma pergunta sobre três domínios leva 5 chamadas e cerca de 9K tokens com subagentes, e 7+ chamadas e cerca de 14K+ com repasses. No laboratório você executa um mesmo trabalho dos dois jeitos e conta.

## Experimente: Uma equipe de equipes

Três etapas: execute uma equipe de equipes e leia o stream dela, pause dentro de uma equipe para abrir o estado dela e depois execute o mesmo trabalho como subagentes e como repasses.

https://agenticademy.org/pt/paths/advanced-agents/03-subgraphs-and-teams

## Verificação

A equipe compartilha messages com o grafo de cima; notes é a chave própria dela. O que o loop imprime?

Código:

```
const Team = Annotation.Root({
  ...MessagesAnnotation.spec,
  notes: Annotation({ reducer: (a, b) => a.concat(b), default: () => [] }),
});
const team = new StateGraph(Team)
  .addNode("work", () => ({ notes: ["n1"], messages: [{ role: "assistant", content: "done" }] }))
  .addEdge(START, "work")
  .compile();
const top = new StateGraph(MessagesAnnotation)
  .addNode("team", team)
  .addEdge(START, "team")
  .compile();

const input = { messages: [{ role: "user", content: "go" }] };
for await (const [ns, update] of await top.stream(input, { streamMode: "updates", subgraphs: true })) {
  const [node, values] = Object.entries(update)[0];
  console.log(ns.length ? "inside" : "top", node, Object.keys(values));
}
```

- **✓** inside work ["notes", "messages"], depois top team ["messages"]: primeiro a atualização própria da equipe, depois o que volta para o grafo de cima.
- top team ["notes", "messages"]: o stream mostra só o grafo de cima, e as chaves do subgrafo voltam com ele.
- inside work ["notes", "messages"], depois top team ["notes", "messages"]: toda chave que a equipe escreveu volta.
- Lança um erro: o grafo de cima não tem a chave notes, então a equipe não pode escrevê-la.

**Por quê:** Com subgraphs: true, o stream também produz as atualizações próprias do subgrafo, sob o namespace dele ["team:<task id>"]; sem isso você veria só a linha top. A equipe é o próprio nó, então as chaves que ela compartilha voltam: messages. notes existe só dentro da equipe, então fica lá, e escrevê-la lá é permitido.

## O que você já sabe fazer

- Adicionar um subgrafo como nó quando ele compartilha chaves, ou chamá-lo a partir de um nó envoltório que converte o estado.
- Fazer streaming com subgraphs: true e ler o namespace de cada atualização; ler o estado de um subgrafo pausado com getState.
- Passar o controle para um nó do grafo pai com Command.PARENT, e construir supervisores de supervisores.
- Escolher entre subagentes, repasses, skills, um router e um fluxo personalizado pelas chamadas ao modelo e pelos tokens.

---

[Voltar para Agentes avançados](README.md) · [Próximo: Viagem no tempo](04-time-travel.md)
