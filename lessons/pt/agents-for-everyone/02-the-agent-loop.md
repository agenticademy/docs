# O loop: olhar, decidir, agir

Agentes para todos · Capítulo 2 de 8 · 12 minutos

Todo agente roda o mesmo loop: olha o que sabe, decide o próximo passo, age e olha de novo.

## Objetivos

- Tome três decisões como se você fosse o modelo.
- Leve o agente até o objetivo.
- Coloque em ordem os passos de uma volta.

## A leitura

### Um loop, de novo e de novo

No capítulo 1 você viu um agente trabalhar em passos. Cada passo segue o mesmo loop de três partes.

Olhar: o agente reúne o que tem até agora, que é o seu pedido e o resultado do último passo. Decidir: o modelo lê tudo isso e escolhe o próximo passo, ou decide que o objetivo foi alcançado. Agir: o agente dá o passo, geralmente usando uma ferramenta, e um resultado volta. Então ele olha de novo.

Uma passagem por olhar, decidir e agir se chama uma volta. Um trabalho pequeno pode levar três ou quatro voltas; um grande pode levar dezenas.

### Por que ele olha de novo

O que uma ferramenta devolve é informação nova. Se uma busca mostra que um encanador está fora o mês inteiro, a próxima decisão pode levar isso em conta. É assim que um agente muda de rumo sem que ninguém mande: ele lê o resultado antes de escolher de novo.

### Quando ele para

Um agente para por um de três motivos. O objetivo foi alcançado. Ele precisa de você: faz uma pergunta ou pede sua aprovação, e espera. Ou ele atinge um limite, como um número máximo de voltas.

### Como os loops dão errado

Uma decisão ruim custa uma volta. Reservar antes de encontrar alguém, por exemplo, não leva a nada. Pior é um loop descontrolado: o agente repete o mesmo passo, recebe o mesmo resultado e repete de novo. Sem um limite, isso poderia continuar para sempre, e cada volta usa o modelo, o que leva tempo e muitas vezes custa dinheiro. Por isso agentes reais contam as voltas e param num limite.

### Sua vez

Abaixo, você faz o papel do modelo. O agente olha, depois você decide, depois o agente age e mostra o que voltou. Experimente uma ou duas escolhas ruins e veja quanto elas custam.

## Experimente: Você é o modelo

Você decide cada passo pelo agente. Veja o que cada ação devolve e como uma escolha ruim custa uma volta a mais.

https://agenticademy.org/pt/paths/agents-for-everyone/02-the-agent-loop

## Verificação

Coloque em ordem as partes de uma volta do loop.

1. Ler o pedido e o resultado do último passo.
2. Decidir o próximo passo, ou decidir que o objetivo foi alcançado.
3. Usar uma ferramenta para dar esse passo.
4. Ler o que a ferramenta devolveu.

**Por quê:** Olhar, decidir, agir e olhar de novo: o que a ferramenta devolve passa a fazer parte do que o agente olha na volta seguinte.

## O que você já sabe fazer

- Descrever o loop que todo agente roda: olhar, decidir, agir.
- Citar três motivos pelos quais um agente para: objetivo alcançado, ele precisa de você ou um limite.
- Explicar por que agentes precisam de um limite de passos.

---

[Voltar para Agentes para todos](README.md) · [Próximo: Ferramentas: como um agente alcança o mundo](03-tools.md)
