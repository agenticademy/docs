# Tokens

Entendendo os modelos · Capítulo 2 de 8 · 14 minutos

Os modelos leem e escrevem texto como tokens, cada família de modelos divide o texto do seu jeito, e tanto os preços quanto os limites de contexto são contados em tokens.

## Objetivos

- Divida uma frase com os tokenizadores de seis famílias de modelos.
- Mude para espanhol, japonês e código, e compare as contagens.
- Passe na verificação.

## A leitura

### O texto vira tokens

No capítulo anterior, você conheceu muitos tipos de modelo. Os que trabalham com texto começam todos do mesmo jeito. Um tokenizador, um pequeno programa que vem com cada modelo, corta o texto em tokens: pedaços que muitas vezes são uma palavra inteira com o espaço antes dela, e às vezes parte de uma palavra, um sinal ou um dígito. Depois, cada token é trocado por um número, o seu ID de token. O modelo lê só esses números. Quando escreve, ele devolve IDs de token, e o tokenizador os transforma em texto.

### Cada família tem o seu próprio vocabulário

A lista de pedaços que um tokenizador conhece é o seu vocabulário. Ela é montada a partir de uma grande amostra de texto antes de o modelo ser treinado: os pedaços que aparecem muito ali viram um token cada. Cada família de modelos, como GPT, Llama ou Qwen, tem o seu próprio tokenizador, e uma nova geração pode ganhar um novo. Os seis do lab conhecem entre 32.768 e 262.144 pedaços.

Uma palavra que não está no vocabulário é montada com pedaços menores. Bem no fim estão os bytes, os pequenos códigos que um computador usa para guardar cada caractere. Um caractere japonês ou um emoji ocupa três ou quatro bytes, então um tokenizador que não o tem precisa de vários tokens para um só caractere.

### Por que a contagem importa

Os provedores cobram pelos seus modelos por milhão de tokens: os tokens que você envia, chamados de entrada, e os tokens que o modelo escreve, chamados de saída. A janela de contexto de um modelo, o máximo de texto que ele consegue receber de uma vez, também é contada em tokens. Um texto que precisa de mais tokens custa mais e enche a janela mais cedo. No lab, o mesmo pedido usa mais tokens em espanhol e em japonês do que em inglês em todas as famílias, e, no código e nos emoji, cabem menos caracteres em cada token do que no inglês.

### O que há no lab

Seis tokenizadores: o da OpenAI para o GPT-4o, um modelo fechado, e os de cinco famílias com pesos abertos, cujos arquivos de modelo são publicados: Gemma 4, do Google, Llama 3, da Meta, Qwen3, DeepSeek-V3 e Mistral v3. As divisões deles foram gravadas com os seus arquivos de tokenizador reais. A última etapa é opcional: ela baixa os arquivos que você escolher, mostrando antes o tamanho deles, e divide o seu próprio texto nesta página.

## Experimente: Lab de tokenizadores

Divida uma frase com seis tokenizadores, depois troque o texto e veja as contagens mudarem.

https://agenticademy.org/pt/paths/understanding-models/02-tokens

## Verificação

Esta palavra alemã significa “mensagem de confirmação de reserva de mesa”. O GPT-4o divide a frase em inglês do lab em 10 tokens, mais ou menos um por palavra. Em quantos ele divide esta única palavra?

A palavra:

```
Tischreservierungsbestätigungsnachricht

1 palavra, 39 letras
Tokens do GPT-4o: ?
```

- 1, porque é uma palavra só.
- **✓** 9, porque ela é montada com pedaços mais longos que estão no vocabulário.
- 39, um para cada letra.
- Nenhum, porque um modelo deixa de fora as palavras que nunca viu.

**Por quê:** O GPT-4o não tem um token único para esta palavra rara, então o tokenizador dele a monta com 9 pedaços que conhece: T, isch, reserv, ierungs, best, ät, igungs, nach, richt. Não é 1, porque só palavras comuns ganham um token próprio. Não é 39, porque sequências de letras que aparecem muito são guardadas como um só pedaço. E nada fica de fora: qualquer texto pode ser montado com pedaços, até chegar a bytes soltos. Outras famílias a dividem de outro jeito: o Mistral v3 precisa de 13 tokens.

## O que você já sabe fazer

- Explicar o que são um tokenizador, um token e um vocabulário.
- Dizer por que o mesmo texto pode virar um número diferente de tokens em duas famílias de modelos.
- Explicar por que os preços e as janelas de contexto dependem da contagem de tokens, e por que outros idiomas, código e emoji costumam custar mais.

---

[Voltar para Entendendo os modelos](README.md) · [Próximo: Escolher o próximo token](03-next-token.md)
