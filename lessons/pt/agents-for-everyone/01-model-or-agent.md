# Um modelo responde. Um agente age.

Agentes para todos · Capítulo 1 de 8 · 10 minutos

A diferença entre um modelo de IA, que devolve texto, e um agente, que dá passos para alcançar um objetivo.

## Objetivos

- Envie o mesmo pedido a um modelo e a um agente, e deixe os dois terminarem.
- Encontre o passo em que o agente mudou algo fora da conversa.
- Passe na verificação.

## A leitura

### Um modelo escreve

Um modelo de IA, como o que está por trás de um chatbot, é um programa treinado com uma quantidade enorme de texto. Dadas algumas palavras, ele prevê as palavras que deveriam vir em seguida. Isso basta para responder a uma pergunta, resumir uma carta ou rascunhar um e-mail.

Os modelos são feitos por empresas e grupos de pesquisa. Alguns modelos são abertos: qualquer pessoa pode baixá-los e executá-los. Muitos são compartilhados no Hugging Face, um site para compartilhar modelos, e alguns, como a família Nemotron da NVIDIA, podem ser testados de graça, com limites.

Sozinho, um modelo só consegue devolver texto. Ele não pode olhar sua agenda nem reservar nada. O que quer que ele sugira, você ainda precisa fazer.

### Um agente faz

Um agente é um programa construído em torno de um modelo. Você dá a ele um objetivo, e ele pode usar ferramentas: outros programas que consultam ou mudam coisas, como uma agenda, um mapa ou um site de reservas.

O agente trabalha em passos. O modelo decide o que fazer em seguida, o agente usa a ferramenta, e o modelo lê o que voltou. Isso se repete até o objetivo ser alcançado, ou até o agente precisar de você. “IA agêntica” é o nome geral para sistemas que funcionam assim.

### Mesmo pedido, dois resultados

Peça a um modelo para reservar uma mesa para seis no sábado e ele explica como reservar. Peça a um agente e ele pode verificar sua agenda, encontrar um restaurante, pedir sua confirmação e fazer a reserva. A simulação abaixo mostra os dois, para você comparar.

### Por que a diferença importa

Texto é fácil de ignorar. Ações não são: uma reserva, um pagamento ou um e-mail enviado têm efeitos no mundo. Por isso um bom agente pergunta antes de fazer algo que importa, e por isso esta trilha dedica um capítulo inteiro à segurança.

### Quatro palavras para esta trilha

Modelo: o programa que lê e escreve texto. Ferramenta: algo que um agente pode usar para consultar coisas ou para agir. Objetivo: o que você pediu ao agente. Passo: uma decisão do modelo e uma ação do agente.

## Experimente: Um pedido, dois resultados

Faça o mesmo pedido a um modelo e depois a um agente. Responda ao agente quando ele perguntar e depois encontre o passo que mudou algo no mundo.

https://agenticademy.org/pt/paths/agents-for-everyone/01-model-or-agent

## Verificação

### Pergunta 1 de 4

Você pede uma receita a um chatbot e ele a escreve. Isso é um agente trabalhando?

- Sim, porque usou IA.
- **✓** Não. Ele devolveu texto, e nada foi feito por você.
- Sim, porque entendeu o que você quis dizer.

**Por quê:** Escrever uma receita é o que um modelo faz sozinho. Um agente usaria ferramentas para agir, por exemplo verificar o que está no seu pedido de mercado online e adicionar os itens que faltam.

### Pergunta 2 de 4

O que um agente tem que um modelo sozinho não tem?

- Uma memória maior da internet.
- **✓** Ferramentas que pode usar e um loop de passos que continua até o objetivo ser alcançado.
- Uma voz e um rosto.

**Por quê:** Um agente é um modelo mais ferramentas mais um loop: decidir, agir, ler o resultado, decidir de novo. O modelo lá dentro pode ser exatamente o mesmo que um chatbot usa.

### Pergunta 3 de 4

Na simulação, por que o agente perguntou a você antes de reservar?

- **✓** Uma reserva tem efeitos reais, então uma pessoa deve confirmá-la primeiro.
- Ele não sabia como reservar.
- Agentes perguntam antes de cada passo.

**Por quê:** Consultar coisas é inofensivo; reservar não é. Bons agentes perguntam antes de ações difíceis de desfazer ou que comprometem você com algo, e cuidam do resto sozinhos.

### Pergunta 4 de 4

Qual destes é uma ferramenta, no sentido usado aqui?

- O texto com que o modelo foi treinado.
- Seu pedido.
- **✓** Uma agenda que o agente pode ler.

**Por quê:** Uma ferramenta é algo que o agente pode usar durante seus passos para consultar coisas ou para agir. Seu pedido é o objetivo, e o texto de treinamento é aquilo com que o modelo aprendeu.

## O que você já sabe fazer

- Distinguir um modelo de um agente com uma pergunta: ele pode agir, ou só responder?
- Nomear as partes de um agente: um modelo, ferramentas, um objetivo e um loop de passos.
- Explicar por que um agente deve perguntar a você antes de uma ação que importa.

---

[Voltar para Agentes para todos](README.md) · [Próximo: O loop: olhar, decidir, agir](02-the-agent-loop.md)
