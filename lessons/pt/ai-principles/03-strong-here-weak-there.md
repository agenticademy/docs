# Forte aqui, fraca ali

Princípios da IA · Capítulo 3 de 8 · 15 minutos

A IA faz bem algumas tarefas difíceis e falha em outras que parecem fáceis, pode afirmar coisas falsas com confiança e é mais útil quando diz que não sabe.

## Objetivos

- Responda a seis perguntas ou diga “Não sei” e depois pontue as respostas de dois jeitos.
- Coloque seis tarefas do dia a dia em um mapa datado de onde a IA vai bem e onde falha.
- Passe na verificação.

## A leitura

### Desigual, e não pela dificuldade

No capítulo 2, você viu um modelo escrever escolhendo os próximos pedaços de texto mais prováveis, aprendidos dos exemplos com que foi treinado. O que ele faz bem depende desses exemplos, não do que as pessoas acham difícil. Ele consegue traduzir uma placa para três idiomas em segundos e depois contar errado as letras de uma palavra, porque lê pedaços de palavras, e não letras soltas.

Pesquisadores chamam isso de fronteira irregular: o limite entre o que a IA faz bem e o que ela faz mal anda em zigue-zague. A expressão vem de um estudo de 2023 em que consultores usaram IA em tarefas realistas (Dell'Acqua e colegas). Nas tarefas dentro da fronteira, o trabalho deles melhorou. Em uma tarefa logo fora dela, quem usava IA acertou menos vezes do que quem trabalhava sem ela. A fronteira também se move à medida que os modelos mudam, então qualquer mapa dela precisa de uma data.

### Confiante e errado

Um modelo sempre produz uma resposta, e as palavras dele soam igualmente seguras quando ele erra. Escrever algo falso em tom confiante, como uma citação de um livro que não existe, se chama confabulação; muita gente chama de alucinação. Estar calibrado significa que a sua confiança corresponde à frequência com que você acerta: quando você diz que tem certeza, quase sempre acerta.

### Por que chutar compensa na maioria dos testes

Abster-se significa dizer “Não sei” em vez de responder. Se isso compensa depende da regra de pontuação, a forma como um teste transforma respostas em pontos. Se só as respostas certas contam, um chute só pode ajudar, então quem sempre responde ganha de quem admite dúvida. Um artigo de 2025 de pesquisadores da OpenAI argumenta que a maioria dos testes usados para ranquear modelos de IA pontua assim, então um modelo que chuta fica acima de um que admite não ter certeza. A correção que ele propõe: fazer uma resposta errada custar mais do que “Não sei”.

### No visual

Você responde a seis perguntas; ninguém poderia saber algumas das respostas a partir desta página. Suas respostas são pontuadas dos dois jeitos, ao lado de dois jogadores simulados. Depois você coloca seis tarefas do dia a dia em um mapa datado: geralmente tudo bem, confira com cuidado ou não confie nisso.

## Experimente: Responder ou se abster, e depois mapear as tarefas

Responda a seis perguntas, veja-as pontuadas de dois jeitos e depois coloque seis tarefas do dia a dia em um mapa datado.

https://agenticademy.org/pt/paths/ai-principles/03-strong-here-weak-there

## Verificação

### Pergunta 1 de 3

Um quiz dá 1 ponto por resposta certa, 0 por “Não sei” e menos 1 por resposta errada. Você tem cerca de 30% de certeza da sua resposta. O que pontua mais em média?

- Responder mesmo assim: um chute vale a pena com qualquer regra de pontuação.
- **✓** Dizer “Não sei”: com 30% de certeza, um chute perde pontos em média.
- Tanto faz: os dois pontuam igual em média.
- Responder e acrescentar uma explicação mais longa para ganhar mais.

**Por quê:** Responder rende 0,3 × 1 + 0,7 × (−1) = −0,4 ponto em média, e “Não sei” rende 0, então os dois não são iguais e se abster pontua mais. Um chute só vale a pena quando respostas erradas não custam nada, e uma explicação mais longa não muda os pontos. O mesmo vale para um modelo pontuado assim.

### Pergunta 2 de 3

Um assistente de chat traduz bem uma carta longa para o japonês e depois erra o número de letras de uma palavra curta. O que isso mostra?

- Ele está quebrado, então não dá para confiar em nada do que faz.
- Contar letras é mais difícil do que traduzir, para as pessoas também.
- Ele não se esforçou na tarefa fácil, como faria uma pessoa entediada.
- **✓** O que ele faz bem não segue o que as pessoas acham difícil.

**Por quê:** Esta é a fronteira irregular: um modelo pode fazer bem uma tarefa difícil para pessoas e escorregar em uma que uma criança consegue fazer, porque lê pedaços de palavras, não letras. Então julgue cada tipo de tarefa por si só. Um deslize não torna todo o trabalho dele indigno de confiança, contar não é mais difícil para as pessoas e ele não estava fazendo corpo mole: o deslize vem de como ele lê o texto.

### Pergunta 3 de 3

Você pergunta a um assistente de chat sem busca na web se a padaria da sua rua está aberta agora. Ele responde: “Sim, ela está aberta até as 18h hoje.” O que você deve fazer?

- **✓** Tratar como um chute e conferir a página da própria padaria, ou ligar.
- Confiar: ele parece seguro, então deve saber.
- Perguntar de novo: se ele disser a mesma coisa duas vezes, está certo.
- Confiar, porque a resposta é curta e específica.

**Por quê:** Ele não tem como ver o horário de hoje, então uma resposta segura e específica ainda pode ser confabulação: as palavras de um modelo soam iguais, saiba ele ou não. Perguntar de novo só traz outro chute, que pode ou não bater. Confira uma fonte que de fato sabe.

## O que você já sabe fazer

- Explicar por que a IA pode fazer bem uma tarefa difícil e falhar em uma que parece fácil.
- Dizer por que uma resposta confiante ainda pode estar errada, e por que pontuar só as respostas certas premia o chute.
- Para uma tarefa do dia a dia, decidir se o resultado da IA geralmente está bom, precisa ser conferido ou não merece confiança.

---

[Voltar para Princípios da IA](README.md) · [Próximo: O que delegar](04-what-to-hand-over.md)
