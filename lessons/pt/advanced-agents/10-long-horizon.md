# Agentes de longo horizonte

Agentes avançados · Capítulo 10 de 10 · 25 minutos

Um trabalho que se estende por várias sessões precisa de compactação dentro de uma sessão, arquivos de passagem e memória entre sessões, e uma verificação de toda a sua trajetória.

## Objetivos

- Execute a sessão 1 e leia os arquivos que ela deixa para a próxima sessão.
- Inicie a sessão 2 só a partir dos arquivos e veja a compactação substituir as mensagens mais antigas por um resumo.
- Avalie as chamadas de ferramentas das sessões de código e abra uma regra que a sessão 3 quebrou.
- Passe na verificação.

## A leitura

### Mais longo que um contexto

Um trabalho de longo horizonte precisa de mais chamadas ao modelo do que cabem em uma janela de contexto, ou de mais de uma sessão: construir um app funcionalidade por funcionalidade, por exemplo. A janela de contexto é o máximo que um modelo lê em uma chamada, contado em tokens. Aqui, uma sessão é uma execução do agente em um thread novo. Duas coisas mantêm esse trabalho no rumo: encolher o contexto dentro de uma sessão e passar o trabalho adiante entre sessões.

### Descarregar em arquivos e compactação

O post da LangChain sobre gestão de contexto no Deep Agents (2026-01-28) descreve três camadas. Um resultado de ferramenta acima de 20.000 tokens vai para um arquivo, e o agente recebe o caminho e uma prévia. Depois de 85% da janela, as entradas de escritas e edições antigas de arquivos viram referências aos seus arquivos. Quando isso não basta, vem a compactação: um modelo escreve um resumo da intenção, dos artefatos e dos próximos passos, o resumo substitui as mensagens mais antigas, e a transcrição completa fica em disco. O harness recriado deste lab descarrega em arquivos a partir de 20.000 tokens e compacta em 85% da janela (summarize: { window, fraction, keep }), sem a camada do meio.

### Arquivos de passagem e memória

O post da Anthropic sobre harnesses para agentes de longa duração (2025-11-26) começa cada trabalho com uma sessão inicializadora. Ela escreve uma lista de funcionalidades em JSON com todas falhando, um arquivo de progresso, um script init.sh e um primeiro commit no git. Cada sessão seguinte lê o arquivo de progresso, o log do git e a lista de funcionalidades, roda um teste básico de ponta a ponta, trabalha em uma funcionalidade, muda o campo passes dela para true só depois de testá-la, e termina com um commit e uma nota de progresso. Nada mais na lista muda. Para fatos de que todo thread precisa, o Store do LangGraph guarda memória de longo prazo sob um namespace, fora de qualquer thread.

### Reinício ou compactação

Começar uma sessão a partir desses arquivos é um reinício de contexto: um contexto limpo e uma passagem por escrito. A compactação mantém um contexto e o encolhe. No post da Anthropic de 2026-03-24, o Sonnet 4.5 encerrava cedo demais à medida que o contexto enchia, então aquele harness reiniciava; com o Opus 4.5, ele passou para a compactação.

### Avaliar a trajetória

Uma trajetória é a sequência de chamadas de ferramentas que uma execução fez. Uma resposta pode dizer “pronto” quando nenhum teste rodou, então avalie as chamadas com regras em código, como as verificações em código de Sistemas de agentes, capítulo 8. Leia as chamadas no trace: depois da compactação, as mensagens já não contêm as primeiras.

## Experimente: Três sessões, um trabalho

Execute a sessão 1, depois a sessão 2 só a partir dos arquivos dela, depois a sessão 3, e avalie as trajetórias das sessões de código.

https://agenticademy.org/pt/paths/advanced-agents/10-long-horizon

## Verificação

Uma sessão de código de um trabalho longo começa em um thread novo. Coloque os passos dela em ordem, como o harness do post da Anthropic os executa.

1. Ler progress.txt, o log do git e feature\_list.json.
2. Rodar o teste de fumaça para conferir que o app ainda funciona.
3. Escrever o código de uma funcionalidade.
4. Rodar os testes dessa funcionalidade.
5. Mudar passes dessa funcionalidade para true em feature\_list.json.
6. Acrescentar uma linha a progress.txt e fazer um commit.

**Por quê:** O thread novo só sabe o que os arquivos dizem, então lê os arquivos primeiro. O teste de fumaça pega o que a última sessão deixou quebrado antes que código novo entre por cima. Uma funcionalidade passa só quando os próprios testes dela passam no código novo: mudar passes primeiro é o atalho que o verificador apontou na sessão 3. A linha de progresso e o commit são o que a próxima sessão lê.

## O que você já sabe fazer

- Preparar um trabalho longo com uma lista de funcionalidades, um arquivo de progresso e commits, para que cada sessão possa começar pelos arquivos.
- Compactar um contexto com summarize ao atingir um limite, e descarregar em arquivos os resultados grandes de ferramentas.
- Guardar memória de longo prazo em um Store do LangGraph que todo thread lê.
- Avaliar uma execução pelas chamadas de ferramentas, com regras em código lidas do trace.

---

[Voltar para Agentes avançados](README.md)
