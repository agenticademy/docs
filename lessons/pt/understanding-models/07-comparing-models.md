# Comparar modelos

Entendendo os modelos · Capítulo 7 de 8 · 15 minutos

Benchmarks e rankings ajudam a comparar modelos, mas as margens de erro, os ajustes de esforço, o custo, os testes vazados, os testes no limite, o programa do teste e quem o rodou mudam o que uma pontuação significa.

## Objetivos

- Leia um ranking com os seus intervalos e encontre os empates.
- Compare um modelo em cinco ajustes de esforço com uma troca para outro modelo.
- Associe quatro armadilhas dos benchmarks a exemplos.
- Passe na verificação.

## A leitura

### Benchmarks e rankings

Um benchmark é um conjunto fixo de tarefas de teste com um jeito de pontuar as respostas. Um ranking ordena os modelos por essas pontuações. O visual usa dois reais. No Arena, as pessoas enviam um prompt, leem duas respostas de modelos que não conseguem ver e votam na que preferem; milhões de votos viram uma pontuação para cada modelo. A Artificial Analysis, uma empresa independente, roda nove benchmarks por conta própria e os combina em um só número, o seu Intelligence Index.

### Toda pontuação tem uma margem

Uma pontuação que vem de votos, ou de uma amostra de tarefas, é uma estimativa. O Arena a escreve como uma pontuação ± uma margem: a pontuação verdadeira muito provavelmente está nessa faixa, chamada intervalo de 95%. Menos votos dão uma faixa mais larga. Quando as faixas de dois modelos se sobrepõem, o ranking não consegue diferenciá-los, então trate-os como empatados, seja qual for a posição deles.

### Ajustes e custo

Muitos modelos têm um ajuste de esforço, como baixo, médio, alto ou máximo: quanto o modelo pode pensar antes de responder, como no capítulo 3. Mais esforço costuma dar uma pontuação maior, mas o modelo escreve mais tokens, então cada tarefa custa mais e demora mais. A Artificial Analysis também mostra um custo por tarefa: quanto custou rodar o modelo em uma das suas tarefas, em dólares americanos. Uma pontuação diz pouco sem o seu ajuste e o seu custo.

### Armadilhas

Contaminação: as perguntas e respostas do teste vazaram para os dados de treinamento, então um modelo pode se lembrar delas em vez de resolvê-las. Saturação: os melhores modelos tiram quase a nota máxima, então o teste já não os separa. O harness é o programa que roda o teste, com as suas instruções, ferramentas e novas tentativas; um harness melhor pode subir a pontuação do mesmo modelo. A pontuação do próprio criador é obtida do jeito dele e pode diferir de uma independente. Deriva de versão: os testes mudam entre versões, então uma pontuação precisa da sua versão e da sua data.

### No visual

Os números do Arena e da Artificial Analysis são reais, como foram publicados, em um levantamento de 2026-11-03. O eixo de custo do gráfico é logarítmico: de US$ 0,10 a US$ 1 ocupa a mesma largura que de US$ 1 a US$ 10, então cabem tanto os modelos baratos quanto os caros. Os exemplos da última etapa são inventados.

## Experimente: Ler pontuações de benchmarks

Encontre os empates em um ranking real, suba a escada de esforço de um modelo e depois associe quatro armadilhas a exemplos inventados.

https://agenticademy.org/pt/paths/understanding-models/07-comparing-models

## Verificação

### Pergunta 1 de 3

Em um exemplo, o modelo A tira 1.310 ± 8 em um ranking e o modelo B tira 1.302 ± 9. O que você pode dizer?

- A é claramente melhor que B.
- **✓** Os intervalos se sobrepõem, então o ranking não consegue diferenciá-los.
- B é melhor, porque o seu intervalo é mais largo.
- As pontuações devem estar erradas.

**Por quê:** O intervalo de A vai de 1.302 a 1.318, e o de B de 1.293 a 1.311. As faixas se sobrepõem, então a diferença pode ser ruído: trate-os como empatados, e não A como claramente melhor. Um intervalo mais largo significa menos certeza, por exemplo menos votos, não um modelo melhor. E nada indica que as pontuações estejam erradas: são estimativas com as suas margens.

### Pergunta 2 de 3

Um relatório diz que o modelo X tira 5 pontos a mais que o modelo Y em um índice. O que você deveria perguntar antes de trocar para X?

- Nada: 5 pontos é uma vitória clara.
- **✓** Em que ajuste de esforço cada um rodou e quanto cada um custou por tarefa.
- Qual dos dois criadores é maior.
- Qual modelo saiu primeiro.

**Por quê:** No visual, de baixo a máximo moveu cada modelo com cinco ajustes de 20 a 29 pontos, com cerca de 8 a 20 vezes o custo por tarefa. Então uma diferença de 5 pontos pode vir só dos ajustes, e a pontuação maior pode custar muito mais por tarefa. O tamanho do criador e a data de lançamento não dizem nada sobre nenhuma das duas coisas.

### Pergunta 3 de 3

Um post de blog coloca a pontuação do modelo A na versão atual de um teste ao lado da pontuação do modelo B na versão de um ano atrás. O número de A é maior. O que você pode concluir?

- A é melhor, porque o seu número é maior.
- B é melhor, porque foi testado primeiro.
- **✓** Pouco, por enquanto: versões diferentes usam tarefas diferentes, então os dois números não estão na mesma escala.
- A versão mais nova deve ser mais fácil.

**Por quê:** Isso é deriva de versão. Uma versão nova muda as tarefas, quantas são ou como são ponderadas, então as suas pontuações não estão na escala antiga. Em 2026-11-03, por exemplo, o índice da Artificial Analysis estava na versão 5.0.1, com nove avaliações. Compare modelos na mesma versão, consultada na mesma data. Ser testado primeiro não torna um modelo melhor, e nada indica que a versão mais nova seja mais fácil.

## O que você já sabe fazer

- Ler os intervalos de um ranking e dizer quais modelos ele não consegue diferenciar.
- Comparar modelos em ajustes de esforço nomeados, junto com o seu custo por tarefa.
- Identificar contaminação, saturação, efeitos do harness e pontuações do próprio criador em uma afirmação.
- Perguntar a versão, a data e quem rodou o teste antes de confiar em uma pontuação.

---

[Voltar para Entendendo os modelos](README.md) · [Próximo: Escolher um modelo para um agente](08-choosing-for-an-agent.md)
