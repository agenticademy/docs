# O que a IA é, e o que ela não é

Princípios da IA · Capítulo 1 de 8 · 12 minutos

A maioria dos sistemas de IA em uso hoje aprende padrões a partir de exemplos e os usa para inferir um resultado para algo novo. Um sistema assim não é uma mente, nem um banco de dados, nem um mecanismo de busca, e seus exemplos moldam cada resposta.

## Objetivos

- Coloque exemplos de dois tipos e deixe o classificador aprendê-los.
- Teste uma fruta nova e veja quais exemplos decidem.
- Adicione um lote enviesado de exemplos e veja a fronteira se mover.
- Passe na verificação.

## A leitura

### Aprender com exemplos

A maioria dos sistemas de IA em uso hoje são programas que aprendem padrões a partir de exemplos e depois usam esses padrões para produzir um resultado para algo novo. Os exemplos se chamam dados. O resultado pode ser um rótulo como “spam”, um número como as vendas da próxima semana ou a próxima palavra de uma frase.

Chegar a um resultado dessa forma se chama inferência: a resposta vem dos padrões, não de uma consulta a algo guardado.

### Treinamento, depois uso

Um sistema assim tem duas fases. No treinamento, ele recebe muitos exemplos e se ajusta para se encaixar neles. No uso, ele aplica o que aprendeu a entradas que nunca viu. Usá-lo não muda o que ele aprendeu; para isso é preciso outra rodada de treinamento.

### O que ele não é

Não é uma mente: ele calcula resultados a partir de padrões e não quer nada. Não é um banco de dados: um banco de dados devolve só o que foi guardado, enquanto um sistema de IA responde para entradas que ninguém guardou, e pode errar. Não é um mecanismo de busca: ele não busca páginas, embora algumas ferramentas o conectem a um.

### Os exemplos decidem

Um sistema aprende só com seus exemplos, então eles moldam cada resposta. Se os exemplos são enviesados, ou seja, se um tipo ou uma fonte tem muito mais exemplos que o resto, as respostas pendem para o mesmo lado. Um sistema que aprendeu principalmente com fotos tiradas em um só país tende a se sair pior com fotos de outros lugares.

### No visual

Você treina um classificador, um programa que separa coisas em tipos, para distinguir limões de laranjas pelo tamanho e pela cor. Cada fruta é um ponto em um mapa. Para julgar uma fruta nova, ele encontra os 3 exemplos mais próximos dela no mapa, seus vizinhos mais próximos, e responde com o tipo da maioria deles. A linha onde a resposta dele muda de um tipo para o outro é a fronteira dele.

Para este classificador simples, treinar significa guardar os exemplos. Já os modelos grandes ajustam uma quantidade enorme de valores internos enquanto treinam, e guardam esses valores em vez dos exemplos.

## Experimente: Ensine um classificador minúsculo

Coloque limões e laranjas no mapa, teste uma fruta que ele não viu e depois adicione um lote enviesado e veja a fronteira se mover.

https://agenticademy.org/pt/paths/ai-principles/01-what-ai-is

## Verificação

### Pergunta 1 de 3

Um sistema foi treinado com milhares de fotos rotuladas como “gato” ou “cachorro”. Agora ele rotula uma foto que nunca viu. O que ele está fazendo?

- Procurando a foto em um depósito de respostas.
- **✓** Inferindo um rótulo a partir de padrões nos seus exemplos.
- Buscando a mesma foto na web.
- Pensando na foto como uma pessoa pensaria.

**Por quê:** Ele aplica padrões dos seus exemplos rotulados a uma entrada nova: isso é inferência. Um depósito de respostas só poderia devolver fotos que já tinha, uma busca na web encontra páginas que existem, e o sistema tem padrões, não os pensamentos de uma pessoa.

### Pergunta 2 de 3

Uma ferramenta que organiza uma caixa de entrada de suporte aprendeu com muitas mensagens de cobrança e só algumas de entrega. Uma mensagem nova não está clara: pode ser de qualquer um dos dois tipos. Qual é o resultado provável?

- **✓** É mais provável que ela a classifique como mensagem de cobrança.
- É igualmente provável que ela a classifique como qualquer um dos dois, porque viu os dois tipos.
- Ela se recusa a classificar a mensagem.

**Por quê:** Os exemplos dela pendem para cobrança, então perto da fronteira as respostas dela também pendem para esse lado, como o lote de laranjas puxou a fronteira no visual. Ela viu entregas, mas poucas demais para se manterem firmes. E um classificador como o do visual sempre escolhe um dos seus tipos: ele não tem como se recusar.

### Pergunta 3 de 3

No visual, por que o classificador começou a chamar um limão típico de laranja?

- O limão mudou de tamanho e de cor.
- **✓** O lote colocou laranjas mais perto dele, então a maioria dos vizinhos mais próximos dele passou a ser laranja.
- Alguém disse ao classificador que a resposta certa era laranja.
- Mais exemplos sempre deixam um classificador menos preciso.

**Por quê:** O limão e a regra (os 3 mais próximos decidem) continuaram iguais; só os exemplos mudaram. Ninguém deu a resposta a ele. Mais exemplos não são o problema em si: exemplos desbalanceados são.

## O que você já sabe fazer

- Explicar com palavras simples que um sistema de IA infere resultados a partir de padrões nos seus exemplos.
- Diferenciar o treinamento (quando um sistema aprende com seus exemplos) do uso (quando ele aplica o que aprendeu).
- Dizer por que um sistema de IA não é uma mente, nem um banco de dados, nem um mecanismo de busca.
- Explicar por que exemplos enviesados levam a respostas enviesadas.

---

[Voltar para Princípios da IA](README.md) · [Próximo: Como um modelo escreve](02-how-a-model-writes.md)
