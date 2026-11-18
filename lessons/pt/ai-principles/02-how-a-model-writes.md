# Como um modelo escreve

Princípios da IA · Capítulo 2 de 8 · 14 minutos

Um modelo de linguagem escreve um token por vez sorteando de uma lista de probabilidades, a temperatura muda como ele sorteia, e nada nesse passo verifica se as palavras são verdadeiras.

## Objetivos

- Leia as probabilidades para um prompt e deixe o modelo pegar a palavra mais provável.
- Escolha você as próximas cinco palavras.
- Escreva com uma temperatura baixa e com uma alta e veja o texto se desviar.
- Passe na verificação.

## A leitura

### Um pedaço por vez

No capítulo anterior, um classificador aprendeu com exemplos a separar pontos em tipos. Um modelo de linguagem, o tipo de IA por trás dos assistentes de chat, aprendeu com uma quantidade enorme de texto a fazer uma só coisa: dado um texto, dizer o que provavelmente vem a seguir. Ele escreve repetindo esse passo. Ele calcula uma probabilidade para cada próximo pedaço possível, escolhe um, acrescenta ao texto e começa de novo.

### Tokens e probabilidades

Os pedaços se chamam tokens. Um token é um pedaço curto de texto: uma palavra, parte de uma palavra ou um sinal de pontuação. Uma probabilidade é um número de 0% a 100%, e as probabilidades de todos os próximos tokens possíveis somam 100%. Depois de “A cafeteria abre às 8 da”, o token “manhã” recebe uma probabilidade alta e “lua”, uma minúscula. O texto do qual você parte se chama prompt.

### Temperatura

O modelo sorteia o próximo token, com peso dado pelas probabilidades, então os tokens prováveis saem com mais frequência. A temperatura é um ajuste que remodela as probabilidades antes do sorteio. Perto de 0, o token mais provável quase sempre vence, e o mesmo prompt dá quase o mesmo texto toda vez. Mais acima, tokens improváveis são escolhidos com mais frequência: o texto varia mais e, a partir de certo ponto, desanda em algo sem sentido.

### Fluente não é o mesmo que verdadeiro

Cada token é escolhido porque é provável que venha a seguir, não porque é verdadeiro. Nada nesse passo verifica um fato. Um modelo pode escrever uma frase fluente com uma data, um preço ou um nome errado, e uma temperatura baixa não resolve isso: ela só escolhe as palavras mais prováveis. Verificar o que ele escreve continua sendo trabalho seu, e um capítulo mais adiante mostra como.

### O modelo desta página

O explorador abaixo roda um modelo minúsculo que o seu navegador treina quando a página abre, com dez fábulas de Esopo. Os tokens dele são palavras inteiras e sinais de pontuação. Ele olha só para os dois últimos e conta o que veio depois deles nas fábulas. Os modelos por trás dos assistentes de chat usam tokens que podem ser partes de palavras, levam em conta muito mais do texto e têm bilhões de parâmetros, os números que eles ajustam enquanto aprendem. O passo de escolher funciona do mesmo jeito.

## Experimente: Explorador da próxima palavra

Leia as probabilidades para um prompt, escolha você algumas palavras e depois mova a temperatura e veja o texto se desviar.

https://agenticademy.org/pt/paths/ai-principles/02-how-a-model-writes

## Verificação

### Pergunta 1 de 3

Você digita “Nosso trem para o litoral sai às” e um modelo continua o texto. O que ele faz?

- Consulta a tabela de horários e copia a hora.
- **✓** Dá a cada próximo token possível uma probabilidade, sorteia um, acrescenta e depois faz o mesmo de novo.
- Escreve a resposta inteira de uma vez e depois a confere com os fatos.
- Sempre escolhe o token mais surpreendente, para manter o texto interessante.

**Por quê:** Um modelo de linguagem escreve um token por vez, sorteando cada um a partir de probabilidades. Ele não consulta nada, a menos que esteja conectado a uma ferramenta, não escreve tudo de uma vez nem verifica fatos, e os tokens prováveis são os mais escolhidos, não os surpreendentes.

### Pergunta 2 de 3

Você faz a mesma pergunta duas vezes a um modelo com uma temperatura alta. O que deve esperar?

- Exatamente a mesma resposta nas duas vezes.
- **✓** Respostas que podem ser diferentes, porque cada token é sorteado a partir de probabilidades remodeladas.
- Um erro, porque uma temperatura alta não é permitida.
- Respostas mais precisas, porque o modelo tenta mais opções.

**Por quê:** Uma temperatura alta iguala as probabilidades, então cada sorteio pode ir para um lado diferente e duas respostas podem ser diferentes. Perto de 0, elas seriam quase iguais. Uma temperatura alta é um ajuste permitido e não deixa as respostas mais precisas: só torna os tokens improváveis mais comuns.

### Pergunta 3 de 3

Um modelo escreve, com fluência e sem ressalvas: “A cafeteria da Elm Street abre às 7h30 aos domingos.” O que a redação fluente diz sobre o horário?

- Está certo, ou o modelo não escreveria com tanta fluência.
- Está certo se a temperatura era baixa.
- **✓** Nada com certeza: as palavras foram escolhidas porque eram prováveis, então confirme o horário com a cafeteria.
- Está errado, porque os modelos sempre inventam horários.

**Por quê:** Uma redação fluente mostra que os tokens eram prováveis, não que são verdadeiros. Uma temperatura baixa só faz o modelo escolher os tokens mais prováveis, que ainda podem estar errados, e os modelos também não erram sempre. Confira fatos como horários de funcionamento com uma fonte em que você confia.

## O que você já sabe fazer

- Explicar que um modelo de linguagem escreve um token por vez, sorteando cada um a partir de probabilidades.
- Dizer o que a temperatura muda e o que ela não muda.
- Explicar por que uma resposta fluente e confiante ainda pode estar errada.

---

[Voltar para Princípios da IA](README.md) · [Próximo: Forte aqui, fraca ali](03-strong-here-weak-there.md)
