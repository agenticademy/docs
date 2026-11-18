# Entendendo os modelos

Iniciante · Sem código · 8 capítulos · 120 min

Para quem quer saber o que há dentro dos modelos por trás das ferramentas de IA. Você vê como o texto vira tokens, como um modelo escolhe o próximo, como os modelos são feitos, qual é o tamanho deles e o que suas licenças permitem, e depois escolhe um modelo para um agente. Os exemplos combinam modelos fechados com modelos abertos, e cada número vem de um levantamento datado.

## Antes de começar

Nada. Os dois primeiros capítulos de Princípios da IA ajudam.

## No fim você sabe

- Nomear os tipos de modelo que um agente usa e o que cada um faz.
- Explicar tokens, como um modelo escolhe o próximo e o que a temperatura muda.
- Acompanhar como um modelo é feito, dos dados e do pré-treinamento até um arquivo quantizado.
- Distinguir modelos fechados, de pesos abertos e de código aberto, e ler os termos principais de uma licença.
- Ler um ranking com cuidado e escolher um modelo para um agente por capacidade, custo e políticas.

## Capítulos

1. [Tipos de modelo](01-kinds-of-models.md): Modelos de chat, de raciocínio, multimodais e de embedding, rerankers e modelos de fala, de imagem, de guarda e pequenos fazem trabalhos diferentes, e um agente costuma usar vários.
2. [Tokens](02-tokens.md): Os modelos leem e escrevem texto como tokens, cada família de modelos divide o texto do seu jeito, e tanto os preços quanto os limites de contexto são contados em tokens.
3. [Escolher o próximo token](03-next-token.md): Um modelo dá uma pontuação a cada próximo token possível, transforma as pontuações em probabilidades e sorteia um; a temperatura, o top-k e o top-p mudam o sorteio, e os modelos base, instruct e de raciocínio são treinados para escrever de jeitos diferentes.
4. [De onde vêm os modelos](04-where-models-come-from.md): Um modelo é feito em etapas, dos dados e do pré-treinamento ao ajuste fino, ao feedback, à destilação e à quantização, e nos modelos abertos muitas etapas deixam um arquivo que você pode inspecionar.
5. [Tamanho e memória](05-size-and-memory.md): A memória de que um modelo precisa vem dos seus parâmetros e dos bits que guardam cada um, mais espaço para o seu contexto; uma mistura de especialistas usa poucos dos seus parâmetros em cada token, mas mantém todos na memória.
6. [Modelos abertos e fechados](06-open-and-closed.md): Alguns modelos só podem ser usados pelo serviço do criador, outros compartilham os seus pesos com licenças que vão de rígidas a permissivas, e alguns poucos são de código aberto, com os seus dados e o seu código.
7. [Comparar modelos](07-comparing-models.md): Benchmarks e rankings ajudam a comparar modelos, mas as margens de erro, os ajustes de esforço, o custo, os testes vazados, os testes no limite, o programa do teste e quem o rodou mudam o que uma pontuação significa.
8. [Escolher um modelo para um agente](08-choosing-for-an-agent.md): Escolher um modelo é descartar os que falham em um limite rígido e depois pesar capacidade, velocidade, preço por tarefa, contexto e confiabilidade com ferramentas para o trabalho de um agente.

https://agenticademy.org/pt/paths/understanding-models
