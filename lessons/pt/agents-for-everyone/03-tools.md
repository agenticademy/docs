# Ferramentas: como um agente alcança o mundo

Agentes para todos · Capítulo 3 de 8 · 12 minutos

As ferramentas são como um agente consulta coisas e faz coisas acontecerem, e o modelo conhece cada ferramenta só pelo rótulo.

## Objetivos

- Dê ao agente a ferramenta certa para três pedidos.
- Veja uma descrição vaga levar o agente à ferramenta errada.
- Passe na verificação.

## A leitura

### O que é uma ferramenta

Uma ferramenta é um programa que um agente pode usar para fazer algo que não consegue fazer só com texto: ver a previsão do tempo, ler uma agenda, fazer uma conta, enviar um e-mail. Para usar uma, o agente preenche os dados de que a ferramenta precisa (uma cidade e um dia, para uma previsão do tempo) e lê o que volta.

### Ferramentas que leem, ferramentas que mudam coisas

Algumas ferramentas só leem: consultam algo e não mudam nada. Outras escrevem: mudam algo no mundo, como enviar um e-mail ou comprar uma passagem. Um erro com uma ferramenta de leitura custa uma volta. Um erro com uma ferramenta de escrita pode custar muito mais, e por isso agentes cuidadosos perguntam a você antes de usar uma.

### O rótulo é tudo o que o modelo vê

O modelo nunca vê o interior de uma ferramenta. Para cada ferramenta ele recebe um rótulo: um nome, uma descrição curta do que ela faz e os dados de que precisa. Ele escolhe comparando seu pedido com esses rótulos.

Por isso as palavras do rótulo importam tanto quanto a própria ferramenta. Um rótulo claro leva à escolha certa. Um vago leva a erros, mesmo quando a ferramenta certa está ali.

### Menos ferramentas, menos erros

Dê a um agente só as ferramentas de que um trabalho precisa. Menos opções significam menos escolhas erradas, e um agente não pode usar mal uma ferramenta que não tem.

Abaixo, você é o ajudante do agente: entregue a ele a ferramenta certa para três pedidos. Depois mude um rótulo e veja o que o agente faz com ele.

## Experimente: Escolha a ferramenta certa

Entregue ao agente a ferramenta certa para três pedidos; escolha uma ferramenta para ler o rótulo dela. Depois deixe o agente escolher pelos rótulos, torne vago o rótulo do E-mail e envie o pedido de novo.

https://agenticademy.org/pt/paths/agents-for-everyone/03-tools

## Verificação

### Pergunta 1 de 3

O que o modelo sabe sobre uma ferramenta quando decide usá-la?

- Como o código da ferramenta funciona por dentro.
- **✓** O nome, a descrição e os dados de que ela precisa.
- Tudo o que a ferramenta já devolveu.

**Por quê:** O rótulo é tudo o que o modelo recebe. Por isso uma descrição clara importa tanto quanto a própria ferramenta.

### Pergunta 2 de 3

Qual destas é uma ferramenta de escrita, que muda algo?

- Ver a previsão do tempo de amanhã.
- Procurar um restaurante.
- **✓** Enviar um e-mail.

**Por quê:** Enviar um e-mail muda algo fora da conversa e não pode ser desfeito. Consultar o tempo ou restaurantes só lê.

### Pergunta 3 de 3

Por que dar a um agente só as ferramentas de que um trabalho precisa?

- Deixa o agente mais lento, porém mais barato.
- **✓** Menos opções significam menos escolhas erradas, e menos dano se uma acontecer.
- Agentes só conseguem ter três ferramentas.

**Por quê:** Um agente não pode usar mal uma ferramenta que não tem. Menos ferramentas, com rótulos claros, facilitam as boas escolhas e reduzem o custo das ruins.

## O que você já sabe fazer

- Explicar o que é uma ferramenta e distinguir ferramentas de leitura de ferramentas de escrita.
- Explicar por que a descrição de uma ferramenta molda as escolhas do agente.
- Defender que um agente receba só as ferramentas de que precisa.

---

[Voltar para Agentes para todos](README.md) · [Próximo: Memória e contexto](04-memory-and-context.md)
