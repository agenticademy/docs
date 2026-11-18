# Escolher o próximo token

Entendendo os modelos · Capítulo 3 de 8 · 14 minutos

Um modelo dá uma pontuação a cada próximo token possível, transforma as pontuações em probabilidades e sorteia um; a temperatura, o top-k e o top-p mudam o sorteio, e os modelos base, instruct e de raciocínio são treinados para escrever de jeitos diferentes.

## Objetivos

- Veja os 20 próximos tokens mais prováveis para um prompt.
- Mexa na temperatura e no top-p, e sorteie de novo.
- Faça a mesma pergunta a um modelo base e a um modelo instruct.
- Passe na verificação.

## A leitura

### Um token por vez

No capítulo anterior, o texto virou tokens. Um modelo escreve do mesmo jeito: lê todo o texto até ali, escolhe um próximo token, acrescenta-o ao texto e começa de novo. Uma resposta de 100 tokens leva 100 desses passos.

### Pontuações viram probabilidades

A cada passo, o modelo dá a cada token que conhece uma pontuação, chamada logit. O SmolLM2-360M, o pequeno modelo aberto deste capítulo, conhece 49.152 tokens, então dá 49.152 pontuações. Depois, as pontuações são transformadas em chances, também chamadas de probabilidades, que somam 100%. Uma pontuação maior significa uma probabilidade maior: um ponto a mais significa cerca de 2,7 vezes a probabilidade.

### Um sorteio, com ajustes

Então um token é sorteado, de acordo com essas probabilidades, como tirar um bilhete de um saco em que os tokens prováveis têm mais bilhetes. Por isso o mesmo prompt pode receber respostas diferentes. Três ajustes mudam o sorteio. A temperatura divide cada pontuação antes de ela virar probabilidade: abaixo de 1, os tokens prováveis ficam ainda mais prováveis; acima de 1, as probabilidades se igualam. O top-k mantém só os k tokens mais prováveis. O top-p mantém os tokens mais prováveis até que suas probabilidades somem p, então um top-p de 0,9 descarta a longa cauda dos improváveis. O que fica é reescalado de volta para 100%.

### Modelos base e instruct

Um modelo base só aprendeu a continuar texto, então uma pergunta pode receber mais perguntas. Um modelo instruct foi treinado também com conversas, escritas num formato de chat que marca quem está falando. Ele responde e depois escreve um token de fim, um token especial que significa que terminou.

### Modelos de raciocínio

Um modelo de raciocínio escreve primeiro o seu raciocínio, do mesmo jeito, token por token, e depois a resposta. Em 2026-11-03, modelos fechados como Claude Opus 5.5 e GPT-6.1 Sol fazem do raciocínio um ajuste, e abertos como DeepSeek-R1 e Olmo 3 7B Think são lançados como modelos de raciocínio separados.

### No visual

Os números do visual foram gravados com o SmolLM2-360M e a sua versão instruct, ambos do Hugging Face. Se o seu navegador tiver WebGPU, uma forma de as páginas web usarem a placa de vídeo, você também pode baixar o modelo instruct e rodá-lo ao vivo.

## Experimente: Sorteio do próximo token

Escolha um prompt e leia as probabilidades, depois dê forma a elas e sorteie, um token por vez. Por fim, faça a mesma pergunta ao modelo base e ao modelo instruct.

https://agenticademy.org/pt/paths/understanding-models/03-next-token

## Verificação

Você sorteia o próximo token 100 vezes com estes ajustes. O que você obtém?

Os ajustes:

```
Prompt: The capital of France is
Modelo: SmolLM2-360M (base)

Próximo token   Probabilidade
Paris           20,3%
the             19,6%
located         10,5%
situated         4,7%
called           3,7%
… mais 15, depois todo o resto

Temperatura: 1
Top-k:       desligado
Top-p:       0,5
```

- “Paris” todas as 100 vezes: é o token mais provável.
- “Paris” e “the”, cerca de 50 vezes cada.
- **✓** “Paris”, “the” e “located”, cerca de 40, 39 e 21 vezes.
- Qualquer um dos 49.152 tokens, cada um com a frequência que a sua probabilidade indica.

**Por quê:** O top-p mantém os tokens mais prováveis até que suas probabilidades cheguem a p. “Paris” e “the” somam 39,9%, menos que 50%; com “located”, o total é 50,4%, então esses três ficam e o resto é cortado. Os três são reescalados de volta para 100%: cerca de 40%, 39% e 21%. “Paris” toda vez exigiria um ajuste que mantém um só token, como top-k em 1 ou top-p em 0,2, e qualquer token só poderia sair com top-p em 1.

## O que você já sabe fazer

- Explicar como um modelo transforma pontuações em probabilidades e sorteia o próximo token.
- Dizer o que a temperatura, o top-k e o top-p fazem com as probabilidades.
- Distinguir a saída de um modelo base da de um modelo instruct, e dizer o que um modelo de raciocínio acrescenta.

---

[Voltar para Entendendo os modelos](README.md) · [Próximo: De onde vêm os modelos](04-where-models-come-from.md)
