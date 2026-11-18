# Tipos de modelo

Entendendo os modelos · Capítulo 1 de 8 · 14 minutos

Modelos de chat, de raciocínio, multimodais e de embedding, rerankers e modelos de fala, de imagem, de guarda e pequenos fazem trabalhos diferentes, e um agente costuma usar vários.

## Objetivos

- Preencha a primeira vaga do agente de voz: transformar a voz de quem liga em texto.
- Preencha as duas vagas que encontram as notas que respondem à pergunta.
- Preencha as três últimas vagas: escrever a resposta, verificá-la e falá-la.
- Passe na verificação.

## A leitura

### Modelos diferentes, trabalhos diferentes

Um modelo é um programa que aprendeu padrões a partir de exemplos. Modelos aprendem trabalhos diferentes. Cada um recebe algum tipo de entrada, como texto, som ou imagens, e entrega algum tipo de saída. Um agente costuma encadear vários modelos, cada um fazendo o trabalho que faz melhor.

### Modelos que escrevem

Um modelo de chat, também chamado de modelo instruct, segue instruções e escreve uma resposta. Um modelo de raciocínio resolve um problema passo a passo antes de responder: é melhor em problemas difíceis, mas mais lento. Muitos modelos novos fazem as duas coisas, com um ajuste de quanto pensam. Um modelo multimodal também lê imagens, som ou vídeo.

### Modelos que encontram e ordenam

Um modelo de embedding transforma um texto em uma lista de números, de modo que textos com significados parecidos recebem números parecidos. Assim, uma busca pode comparar os números e encontrar notas de significado próximo ao de uma pergunta, rápido, mesmo entre milhares. Um reranker lê a pergunta junto com cada nota que recebe e coloca a melhor primeiro. Isso é mais cuidadoso, mas mais lento, então ele só ordena as poucas notas que a busca encontrou.

### Modelos que ouvem, falam, desenham e verificam

Um modelo de fala para texto escreve o que alguém diz. Um de texto para fala lê um texto em voz alta. Um modelo de imagem cria uma imagem a partir de uma descrição. Um modelo de guarda, também chamado de classificador de segurança, rotula textos ou imagens como seguros ou inseguros, para que um agente possa barrar uma resposta ruim antes que alguém a veja ou ouça.

### Pequenos, fechados e abertos

O tamanho de um modelo é contado em parâmetros: os valores internos que ele ajustou no treinamento, também chamados de pesos. Modelos pequenos têm alguns bilhões de parâmetros ou menos, então podem rodar em um celular ou notebook. Os pesos de um modelo fechado não são publicados, e a maioria dos modelos fechados é usada por meio de um serviço online. Um modelo de pesos abertos pode ser baixado e rodado no seu próprio computador.

No visual, você monta um agente de voz para a linha telefônica de uma biblioteca, uma vaga por vez. A biblioteca é inventada; cada modelo de exemplo que você encontra é real, em 2026-11-03.

## Experimente: Monte um agente de voz

Preencha uma vaga por vez, de cima para baixo: leia o que a vaga recebe e o que precisa entregar, e depois escolha o cartão de modelo que combina.

https://agenticademy.org/pt/paths/understanding-models/01-kinds-of-models

## Verificação

### Pergunta 1 de 3

Um agente de suporte precisa encontrar, entre 5.000, os três artigos de ajuda que melhor correspondem à pergunta de um cliente. Qual par de modelos é feito para esse trabalho?

- Um modelo de imagem e um modelo de fala para texto.
- **✓** Um modelo de embedding para encontrar artigos de significado próximo, e depois um reranker para colocar o melhor primeiro.
- Um modelo de guarda e um modelo de texto para fala.
- Um reranker sozinho, lendo todos os 5.000 artigos com a pergunta.

**Por quê:** Os embeddings permitem que uma busca compare números, então ela encontra artigos parecidos rápido, mesmo entre milhares. Depois, um reranker lê a pergunta com cada um dos poucos encontrados e coloca o melhor primeiro. Um reranker sozinho leria todos os 5.000 pares a cada pergunta, o que é lento. Modelos de imagem, de fala, de guarda e de texto para fala fazem outros trabalhos: imagens, som e rótulos de segurança.

### Pergunta 2 de 3

Um app de trabalho de campo precisa responder perguntas em um celular sem conexão com a internet. Que tipo de modelo consegue fazer isso?

- Um modelo de chat fechado, usado pelo serviço do fabricante.
- **✓** Um modelo pequeno de pesos abertos, baixado no celular.
- Um modelo de raciocínio em um servidor.
- Um modelo de embedding sozinho.

**Por quê:** Sem conexão, o modelo precisa rodar no próprio celular. Um modelo pequeno de pesos abertos consegue: você o baixa uma vez, e ele roda sem internet. Um modelo de chat fechado usado pelo serviço do fabricante e um modelo de raciocínio em um servidor precisam, os dois, de conexão para serem acessados. Um modelo de embedding entrega números, não respostas.

### Pergunta 3 de 3

Alguém liga para um agente de voz, faz uma pergunta fácil e espera na linha. Por que um modelo de chat serve melhor que um modelo de raciocínio para a vaga da resposta?

- Um modelo de raciocínio não consegue ler texto.
- **✓** Um modelo de raciocínio passa por etapas antes de responder, então quem liga espera mais por uma resposta fácil.
- Um modelo de chat está sempre certo.
- Um modelo de raciocínio entrega som, não texto.

**Por quê:** Os dois recebem texto e entregam texto. Um modelo de raciocínio gasta tempo pensando antes de responder, o que ajuda em problemas difíceis, mas deixa quem liga em silêncio diante de uma pergunta fácil. Nenhum modelo está sempre certo, e transformar texto em som é o trabalho do texto para fala.

## O que você já sabe fazer

- Nomear dez tipos de modelo e dizer o que cada um recebe e entrega.
- Escolher um tipo de modelo para cada etapa de um agente simples, e dizer por que outro tipo não serviria.
- Dar um exemplo fechado e um aberto de cada tipo, a partir de um levantamento datado.

---

[Voltar para Entendendo os modelos](README.md) · [Próximo: Tokens](02-tokens.md)
