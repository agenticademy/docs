# Dizer o que você quer

Princípios da IA · Capítulo 5 de 8 · 15 minutos

Onde um pedido deixa uma lacuna, o modelo muitas vezes a preenche com um palpite provável, então um bom pedido descreve o resultado, o jeito de trabalhar e o comportamento que você quer, com os fatos e um exemplo que só você pode dar.

## Objetivos

- Leia um pedido vago e a resposta que ele recebeu, e marque o que a resposta teve de adivinhar.
- Acrescente partes ao pedido uma de cada vez até o verificador não achar lacunas.
- Compare as respostas ao pedido vago e ao pedido completo.
- Passe no teste.

## A leitura

### Uma lacuna ganha um palpite

O capítulo 4 tratava do que delegar. Este trata de como pedir. O que você digita para um assistente de chat de IA é um prompt: o texto do qual o modelo parte, como no capítulo 2. Num chat simples, sem acesso aos seus arquivos ou à sua agenda, tudo o que o modelo sabe sobre a sua situação é o que você escreve.

Onde o seu pedido deixa uma lacuna, o modelo muitas vezes a preenche com o que é provável em geral, do mesmo jeito que escolhe palavras prováveis. O resultado se lê bem, então os palpites passam fácil despercebidos: uma data que ninguém marcou, uma promessa que ninguém fez.

### Três coisas para descrever

Escreva o seu pedido como um briefing: as instruções que você daria a um ajudante capaz que não sabe nada da sua situação. Vale a pena descrever três coisas.

O resultado: para quem é, a forma (uma lista, um aviso, um e-mail) e qual deve ser o tamanho.

O jeito de trabalhar: os passos a seguir e o que perguntar a você antes de começar. “Se faltar um fato, me pergunte” transforma um palpite numa pergunta.

O comportamento: como deve soar e o que evitar, como promessas que você não pode cumprir.

### O que só você sabe

Mais duas partes trazem o que o modelo não pode adivinhar. O contexto são os fatos de fundo: datas, nomes, o que já foi decidido. Um exemplo mostra como é algo bom, como um aviso que funcionou da última vez. Diga o que aproveitar dele, como o layout, ou o modelo pode copiar também a data antiga.

Frases como “capriche” acrescentam tamanho, não informação: não dizem o que é bom.

### No visual

Você começa com um pedido vago de um aviso para uma horta comunitária e a resposta que ele recebeu, e marca o que essa resposta teve de adivinhar. Depois acrescenta partes uma de cada vez enquanto um verificador aponta o que ainda falta. O verificador são algumas regras fixas que rodam nesta página e olham quais partes o seu pedido tem; ele não é uma IA. Por último, você envia o pedido completo e compara as duas respostas. A horta e as duas respostas foram escritas para este capítulo para mostrar o tipo de palpite que um modelo pode dar.

## Experimente: Montador de briefing

Marque o que um pedido vago deixou para adivinhar, acrescente partes até o verificador não achar lacunas e depois compare as duas respostas.

https://agenticademy.org/pt/paths/ai-principles/05-saying-what-you-want

## Verificação

### Pergunta 1 de 4

Você pede a um assistente de chat de IA, que não tem acesso à sua agenda nem ao seu e-mail: “Escreva um lembrete sobre o almoço da equipe na sexta.” A resposta diz que o almoço é às 12:30 no salão principal. Você nunca deu um horário nem um lugar. O que aconteceu?

- Ele encontrou o almoço na sua agenda e copiou de lá o horário e o lugar.
- Ele sabe, pelo treinamento, quando e onde acontecem os almoços de equipe no seu trabalho.
- **✓** O seu pedido deixou lacunas, e o modelo as preencheu com detalhes prováveis.

**Por quê:** O modelo só sabia o que você escreveu. Onde ficou uma lacuna, ele escreveu o que é provável em geral, o que se lê bem e pode estar errado. Ele não podia consultar nada, e não sabe nada sobre o seu trabalho se você não contar.

### Pergunta 2 de 4

Você quer ajuda para planejar um jantar de aniversário para o seu pai. Qual linha, acrescentada ao seu pedido, dá à IA mais coisas para agir?

- **✓** “Oito pessoas, uma delas vegetariana, uns 30 euros por pessoa, perto da estação. Ele gosta de lugares tranquilos.”
- “Faça ser algo muito especial, o tipo de noite de que ele vai se lembrar por anos.”
- “Seja o mais criativo possível e me surpreenda com ideias que eu não teria.”

**Por quê:** A primeira dá fatos que a IA não pode adivinhar: quantas pessoas, quem come o quê, o orçamento, a região e do que ele gosta. “Especial” e “criativo” parecem úteis, mas não dizem o que você quer, então a IA fica de novo adivinhando.

### Pergunta 3 de 4

Qual linha descreve o jeito de trabalhar, e não o resultado nem o comportamento?

- “Menos de 100 palavras, como uma lista curta que caiba numa tela de celular.”
- **✓** “Primeiro liste os fatos que você vai usar e me pergunte sobre os que faltarem.”
- “Use um tom caloroso e simpático, e não prometa nada em meu nome.”

**Por quê:** Listar os fatos primeiro e perguntar sobre as lacunas é um jeito de trabalhar a tarefa. O tamanho e a forma pertencem ao resultado; o tom e o que evitar pertencem ao comportamento. Vale a pena dizer as três coisas; elas respondem a perguntas diferentes.

### Pergunta 4 de 4

Você cola a carta do passeio da escola do ano passado como exemplo e pede a deste ano. A nova carta vem com a data e o preço do ano passado. O que o seu pedido deveria ter dito?

- “Faça melhor que a carta do ano passado, e mais atualizada.”
- Nada mais: um exemplo sozinho mostra ao modelo quais partes manter.
- **✓** “Copie o layout, não os fatos. A data e o preço deste ano estão abaixo.”

**Por quê:** Um exemplo mostra como é algo bom, mas não quais partes manter. Diga o que aproveitar dele, aqui o layout, e dê os fatos novos. “Melhor” e “atualizada” não dizem o que mudar, e um exemplo sozinho pode ser copiado inteiro, com os fatos antigos junto.

## O que você já sabe fazer

- Explicar por que um pedido vago recebe uma resposta cheia de palpites.
- Descrever o resultado, o jeito de trabalhar e o comportamento que você quer de uma IA.
- Acrescentar o contexto e um exemplo que só você pode dar, e dizer o que aproveitar do exemplo.

---

[Voltar para Princípios da IA](README.md) · [Próximo: Conferir o que você recebe](06-checking-what-you-get.md)
