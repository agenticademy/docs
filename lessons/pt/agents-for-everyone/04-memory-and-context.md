# Memória e contexto

Agentes para todos · Capítulo 4 de 8 · 12 minutos

O que um agente consegue ver a cada passo é limitado, então o que ele guarda, descarta e anota decide o que ele sabe.

## Objetivos

- Encha o contexto até as primeiras mensagens saírem.
- Salve o fato certo para o agente responder corretamente depois.
- Passe na verificação.

## A leitura

### O que o modelo lê

Cada vez que o modelo decide um passo, ele lê um bloco de texto chamado contexto: seu pedido, a conversa até agora e o que as ferramentas devolveram. É tudo o que ele tem. Se algo não está no contexto, o modelo não sabe disso naquele passo.

### Uma mesa com espaço limitado

O contexto tem um limite de tamanho, contado em tokens. Um token é um pedaço de palavra: uma palavra curta tem cerca de um token, e em inglês um token tem cerca de quatro caracteres. Os limites variam entre modelos, de alguns milhares de tokens a um milhão ou mais, mas sempre existe um.

Pense no contexto como uma mesa. Só cabe uma certa quantidade de papel nela de cada vez.

### Quando a mesa está cheia

Uma conversa longa enche a mesa. Então algo tem de sair: as mensagens mais antigas são descartadas, ou várias são trocadas por um resumo curto. O que for descartado, o agente deixa de saber, mesmo que você tenha dito antes.

### Memória: um caderno ao lado da mesa

Memória é outra coisa. São notas que o agente escreve em algum lugar fora do contexto, como um arquivo, e relê quando precisa delas. Uma nota sobrevive quando a mesa é limpa, mas só se o agente a salvou, e só a parte que ele escolheu anotar.

### Mais nem sempre é melhor

Um contexto lotado leva mais tempo para ler, custa mais e pode esconder o único fato que importa. Bons agentes deixam na mesa o que o próximo passo precisa e anotam o resto no caderno.

## Experimente: Uma mesa com espaço limitado

Encha a mesa até as mensagens mais antigas saírem e veja o que o agente esquece. Depois comece de novo, salve um fato no caderno e pergunte outra vez.

https://agenticademy.org/pt/paths/agents-for-everyone/04-memory-and-context

## Verificação

### Pergunta 1 de 3

O que está no contexto de um agente em um passo?

- Tudo o que há na internet.
- **✓** O pedido, a conversa até agora e os resultados das ferramentas que ainda cabem.
- Só a sua última mensagem.

**Por quê:** O modelo lê só o que é colocado diante dele naquele passo, até o limite de tamanho.

### Pergunta 2 de 3

O contexto está cheio e chega uma mensagem nova. O que costuma acontecer?

- O agente para de trabalhar de vez.
- **✓** Mensagens mais antigas são descartadas ou resumidas para abrir espaço.
- O modelo ganha um contexto maior.

**Por quê:** O limite é fixo para cada modelo, então o agente precisa abrir espaço. O que ele descarta, deixa de saber, a menos que tenha salvado uma nota.

### Pergunta 3 de 3

Em que a memória é diferente do contexto?

- **✓** Memória são notas guardadas fora do contexto e relidas quando necessário.
- São a mesma coisa.
- Memória é o que o modelo aprendeu no treinamento.

**Por quê:** A memória sobrevive quando o contexto é limpo, porque fica em outro lugar. O agente precisa escolher salvá-la e relê-la.

## O que você já sabe fazer

- Explicar o que é um contexto e por que ele tem limite.
- Dizer o que acontece quando ele enche.
- Distinguir contexto de memória e dizer por que salvar os fatos certos importa.

---

[Voltar para Agentes para todos](README.md) · [Próximo: Planos e agentes trabalhando juntos](05-plans-and-teams.md)
