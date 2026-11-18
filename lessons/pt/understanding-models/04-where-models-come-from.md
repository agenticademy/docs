# De onde vêm os modelos

Entendendo os modelos · Capítulo 4 de 8 · 15 minutos

Um modelo é feito em etapas, dos dados e do pré-treinamento ao ajuste fino, ao feedback, à destilação e à quantização, e nos modelos abertos muitas etapas deixam um arquivo que você pode inspecionar.

## Objetivos

- Acompanhe o Olmo 3, um modelo totalmente aberto, dos dados ao lançamento.
- Acompanhe o DeepSeek-R1 até um modelo pequeno destilado dele.
- Acompanhe o Gemma 4 do modelo base até um arquivo quantizado.
- Passe na verificação.

## A leitura

### Dados, tokens e pré-treinamento

Todo modelo começa com dados: grandes coleções de texto, código e muitas vezes imagens, filtradas e sem cópias repetidas. Um tokenizador, como no capítulo 2, divide tudo em tokens.

O pré-treinamento mostra ao modelo trilhões de tokens e ajusta os seus pesos, também chamados de parâmetros: os números que ele aprende. Ele fica melhor em prever o próximo token, a tarefa do capítulo 3. Os laboratórios salvam checkpoints, cópias dos pesos ao longo do caminho. Alguns, como o Olmo 3, terminam com um treinamento intermediário, uma fase mais curta com uma mistura menor e escolhida, e uma fase com textos longos. O resultado é um modelo base, que continua texto mas não segue pedidos.

### Pós-treinamento

O pós-treinamento faz um modelo base seguir pedidos. O ajuste fino supervisionado (SFT) o treina com exemplos de pedidos com boas respostas.

Depois, o aprendizado por reforço (RL) o treina com recompensas: respostas com boa pontuação ficam mais prováveis. No RLHF, RL com feedback humano, pessoas comparam respostas e as pontuações vêm das escolhas delas. No RLAIF, outro modelo dá o feedback, às vezes guiado por uma lista escrita de princípios chamada constituição. O DPO, otimização direta de preferências, aprende com pares de uma resposta melhor e uma pior, em um único passo mais simples.

Os modelos de raciocínio acrescentam RL com recompensas verificáveis (RLVR): uma resposta só é recompensada quando um programa a confirma, como um resultado de matemática ou um código que passa nos seus testes.

### Menores e mais leves

A destilação treina um modelo pequeno, o aluno, com respostas escritas por um grande, o professor. A quantização guarda cada peso com menos bits, os 0 e 1 da memória de um computador, por exemplo 4 em vez de 16, então o arquivo encolhe e cabe em máquinas menores, com uma pequena perda de qualidade. O treinamento consciente da quantização (QAT) treina um modelo para perder menos quando for quantizado. GGUF é um formato de arquivo lido por apps que rodam modelos no seu próprio computador.

### Testados e descritos

Antes do lançamento, os laboratórios avaliam um modelo em benchmarks, conjuntos fixos de perguntas de teste, e fazem red teaming: testadores tentam fazer o modelo causar dano. Uma ficha de modelo descreve o modelo, o seu treinamento e os seus resultados; uma ficha de sistema também cobre as salvaguardas em volta dele.

Os modelos abertos publicam os seus pesos; os totalmente abertos também publicam os seus dados e o seu código. Modelos fechados como Claude, GPT, Gemini e Grok também são treinados, mas os seus criadores publicam relatórios, não pesos, então as suas árvores não podem ser acompanhadas arquivo por arquivo. O visual acompanha três modelos abertos.

## Experimente: A árvore genealógica de um modelo

Acompanhe três árvores reais a partir do topo, uma etapa por vez. Cada etapa mostra o que usou e o que produziu, e onde isso está publicado.

https://agenticademy.org/pt/paths/understanding-models/04-where-models-come-from

## Verificação

Coloque em ordem as etapas de fazer um modelo, da primeira à última.

1. Reunir e limpar os dados, e dividi-los em tokens.
2. Pré-treinar prevendo o próximo token: o resultado é um modelo base.
3. Fazer o ajuste fino com exemplos de pedidos com boas respostas.
4. Treinar mais com recompensas: de feedback e de respostas que um programa pode verificar.
5. Destilar um modelo aluno menor a partir das respostas dele.
6. Quantizar os pesos do aluno para menos bits.
7. Avaliar o resultado e fazer red teaming, depois publicar a ficha de modelo.

**Por quê:** O pré-treinamento lê tokens, então os dados e os tokens vêm primeiro, e ele gera um modelo base. O ajuste fino com exemplos ensina esse modelo a seguir pedidos, e as recompensas o moldam ainda mais, como no SFT, no DPO e no RLVR do Olmo 3. Só um modelo pronto pode ensinar um aluno, e a quantização então encolhe o arquivo do aluno. Os últimos testes e o red teaming vêm antes do lançamento, e a ficha de modelo os relata.

## O que você já sabe fazer

- Nomear em ordem as etapas que fazem um modelo, dos dados à ficha de modelo.
- Explicar o que o ajuste fino, o treinamento com feedback, a destilação e a quantização mudam, cada um.
- Acompanhar a árvore genealógica de um modelo aberto a partir da ficha de modelo, etapa por etapa.
- Dizer por que um modelo totalmente aberto pode ser verificado em cada etapa e um fechado não.

---

[Voltar para Entendendo os modelos](README.md) · [Próximo: Tamanho e memória](05-size-and-memory.md)
