# Tamanho e memória

Entendendo os modelos · Capítulo 5 de 8 · 15 minutos

A memória de que um modelo precisa vem dos seus parâmetros e dos bits que guardam cada um, mais espaço para o seu contexto; uma mistura de especialistas usa poucos dos seus parâmetros em cada token, mas mantém todos na memória.

## Objetivos

- Leve o Llama-3.1-8B até um tipo de 4 bits e veja o arquivo encolher.
- Verifique quais versões cabem em 8, 16, 24 e 80 GB.
- Compare os parâmetros totais e ativos de três modelos de mistura de especialistas.
- Passe no teste.

## A leitura

### Parâmetros ocupam memória

No capítulo 1 você conheceu os parâmetros, os números que um modelo aprendeu, também chamados de pesos. O nome muitas vezes dá a quantidade: o Llama-3.1-8B tem cerca de 8 bilhões. Para rodar, um modelo precisa manter todos os pesos na memória: o armazenamento rápido com que um computador trabalha, ou a memória de uma placa de vídeo (uma GPU, um chip que faz muitas contas ao mesmo tempo). A memória é contada em bytes, e um byte tem 8 bits. Então os pesos precisam de parâmetros × bits por peso ÷ 8 bytes. Com 16 bits cada, 8 bilhões de pesos ocupam 16 GB, 16 bilhões de bytes.

### Menos bits, um pouco pior

A quantização, do capítulo 4, guarda cada peso com menos bits. Menos bits conseguem guardar menos valores, então cada peso é arredondado de forma mais grosseira: o arquivo encolhe, e as respostas ficam um pouco piores, mais ainda com o menor número de bits. O visual usa medições do llama.cpp, um programa gratuito que roda modelos no seu próprio computador. Os nomes dos seus tipos dão os bits: o Q4\_K\_M guarda a maioria dos pesos em 4 bits e alguns em mais, mais um pouco extra para cada bloco de pesos: 4,89 bits em média. Ele dá os tamanhos em GiB: 1 GiB são 1.073.741.824 bytes, cerca de 7% a mais que 1 GB.

### Espaço para trabalhar

Um modelo cabe quando os seus pesos, mais algum espaço, cabem na memória que você tem. Num notebook, essa memória é dividida com tudo o mais que está rodando. O espaço é para o contexto: enquanto um modelo trabalha, ele guarda o que calculou para cada token, e esse estoque cresce a cada token, até a janela de contexto do capítulo 2.

### Muitos especialistas, poucos de cada vez

Um modelo de mistura de especialistas divide a maioria das suas camadas em especialistas, grupos menores de pesos. Para cada token, uma pequena parte chamada roteador escolhe alguns especialistas, e só eles trabalham. Os parâmetros deles, mais as partes que todo token usa, são os parâmetros ativos; todos juntos são o total. Um token custa mais ou menos o mesmo trabalho que num modelo do tamanho ativo, mas o roteador pode escolher qualquer especialista para o próximo token, então o total precisa estar na memória.

### Fechados e abertos

Modelos fechados como Claude, GPT, Gemini e Grok não publicam os pesos, e as suas páginas não dão a quantidade de parâmetros, então cada quantidade de parâmetros do visual vem da página de um modelo aberto, na data de 2026-11-03.

## Experimente: Será que cabe

Leve um modelo para menos bits, teste quatro tamanhos de memória e depois veja quanto de um modelo grande trabalha em cada token.

https://agenticademy.org/pt/paths/understanding-models/05-size-and-memory

## Verificação

Este modelo de mistura de especialistas está quantizado em cerca de 4,9 bits por peso. Qual é a menor destas memórias que guarda os seus pesos?

O modelo:

```
Modelo:     gpt-oss-120b, pesos abertos
Total:      117 bilhões de parâmetros
Ativos:     5,1 bilhões em cada token
Números:    ficha do modelo, 2026-11-03
Quantizado: 4,9 bits por peso

Memória:    8 GB · 16 GB · 24 GB · 80 GB
```

- 8 GB: só os 5,1 bilhões de parâmetros ativos precisam estar na memória, cerca de 3,1 GB.
- 16 GB: os 5,1 bilhões de parâmetros ativos com 16 bits cada, cerca de 10,2 GB.
- **✓** 80 GB: todos os 117 bilhões de parâmetros precisam estar na memória, cerca de 72 GB.
- Nenhuma delas: 117 bilhões × 4,9 bits dão cerca de 573 GB.

**Por quê:** O roteador pode escolher qualquer especialista para o próximo token, então todos os 117 bilhões de parâmetros precisam estar na memória, não só os 5,1 bilhões que um token usa: 117 bilhões × 4,9 bits ÷ 8 bits por byte ≈ 72 GB, o que deixa cerca de 8 GB dos 80 para o contexto. 8 GB e 16 GB guardariam só a parte ativa, com 4,9 ou com 16 bits. 573 GB esquece de dividir por 8, os bits de um byte.

## O que você já sabe fazer

- Estimar a memória de que os pesos de um modelo precisam a partir dos parâmetros e dos bits por peso.
- Dizer o que a quantização economiza e o que ela custa.
- Dizer se um modelo cabe numa memória dada, com espaço sobrando para o contexto.
- Explicar os parâmetros totais e ativos numa mistura de especialistas, e por que o total define a memória.

---

[Voltar para Entendendo os modelos](README.md) · [Próximo: Modelos abertos e fechados](06-open-and-closed.md)
