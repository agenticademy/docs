# Escolher um modelo para um agente

Entendendo os modelos · Capítulo 8 de 8 · 18 minutos

Escolher um modelo é descartar os que falham em um limite rígido e depois pesar capacidade, velocidade, preço por tarefa, contexto e confiabilidade com ferramentas para o trabalho de um agente.

## Objetivos

- Escolha um agente para o qual escolher um modelo.
- Aplique os limites rígidos dele e veja quais modelos saem.
- Pese o que sobrou, ordene e calcule o preço de um mês de trabalho.
- Passe na verificação.

## A leitura

### O que serve, não o melhor

Não existe o melhor modelo, só o que melhor serve ao trabalho de um agente. Escolher leva duas passadas. Primeiro vêm os limites rígidos: necessidades sem as quais o agente não funciona. Um modelo que falha em um deles sai, por mais forte ou barato que seja. Depois você pesa o que sobrou: decide quanto cada qualidade importa para esse trabalho e ordena os modelos por isso.

### Limites rígidos

Modalidade: os tipos de entrada e saída que um modelo trata, como texto, imagens ou som. Janela de contexto: quantos tokens ele lê de uma vez, como no capítulo 2. Política de dados: se o provedor pode treinar os modelos dele com o que você envia, e a retenção, por quanto tempo ele guarda isso. Região: onde os seus dados são processados; algumas regras, como as da UE, se importam com o lugar. Licença e acesso: o que você pode fazer com um modelo e quem pode usá-lo. Pesos abertos também podem rodar na sua própria máquina, até sem internet.

### O que você pesa

Capacidade: quão bem ele faz o trabalho. Benchmarks, como no capítulo 7, ajudam quando as tarefas deles são parecidas com as suas, mas a sua própria avaliação diz mais: algumas dezenas de tarefas reais do trabalho, pontuadas por você. Latência: quanto você espera por uma resposta; pensar mais costuma dar respostas melhores e esperas mais longas.

Preço por tarefa: quanto custa uma tarefa concluída. Um agente chama o modelo várias vezes por tarefa, e cada chamada é um turno, cobrado pelos tokens de entrada e de saída. Um preço por token baixo ainda pode dar uma tarefa cara. Confiabilidade nas chamadas de ferramenta: com que frequência o modelo chama ferramentas corretamente, com as entradas certas. Limites de requisições: quantas requisições e tokens um provedor aceita por minuto.

### No visual

Você escolhe um de quatro agentes. Os limites rígidos dele removem modelos, as suas prioridades ordenam o resto e uma calculadora transforma preços de tabela reais em uma conta mensal. Depois, uma resposta comentada mostra do que a escolha dela abre mão. Os agentes e o tamanho das tarefas deles são inventados. Os modelos, preços, janelas de contexto, entradas, licenças e pontuações são reais, de um levantamento de 2026-11-03. O levantamento não tem regiões e só alguns termos de dados, então alguns limites dizem “Verificar”; ele não tem velocidades nem limites de requisições, então isso você testa por conta própria.

## Experimente: Escolha um modelo para este agente

Escolha um agente, aplique os limites rígidos dele um por um, pese o que sobrou e calcule o preço de um mês de trabalho; depois compare com uma resposta comentada.

https://agenticademy.org/pt/paths/understanding-models/08-choosing-for-an-agent

## Verificação

### Pergunta 1 de 3

Um bot de suporte para um banco europeu precisa manter os dados de clientes na UE. Dois modelos podem fazer o trabalho; o mais barato só roda em servidores fora da UE. O que vem primeiro?

- Escolher o mais barato: o preço é o que mais importa.
- **✓** Descartar o que não consegue manter os dados na UE e depois comparar o resto.
- Escolher o que tem a maior pontuação no ranking.
- Usar os dois e deixar que votem.

**Por quê:** Manter os dados na UE é um limite rígido: um modelo que falha nele sai, por mais barato ou forte que seja. Preço e pontuações só ordenam os modelos permitidos. Deixar os dois votarem ainda enviaria os dados para fora da UE.

### Pergunta 2 de 3

O modelo B custa o dobro por token do modelo A. Na sua avaliação, B conclui uma tarefa em 2 turnos, e A precisa de 5 turnos do mesmo tamanho. Qual custa menos por tarefa?

- A, porque o preço por token dele é metade do de B.
- **✓** B, porque 2 turnos pelo dobro do preço custam menos que 5 turnos.
- Nenhum: com os mesmos tokens por turno, o preço por token decide.
- Não dá para saber sem uma pontuação de ranking.

**Por quê:** O preço por tarefa é o preço por token vezes os tokens que uma tarefa usa. Se um turno custa 1 em A, custa 2 em B, então a tarefa de A custa 5 e a de B custa 4. O preço por token sozinho ignora o número de turnos, e uma pontuação de ranking nem conta os seus turnos; a sua própria avaliação conta.

### Pergunta 3 de 3

O primeiro modelo da sua lista curta tem a melhor pontuação independente, mas o levantamento não diz nada sobre a velocidade dele nem sobre quão bem ele chama ferramentas. O que você faz antes de se comprometer?

- Nada: a melhor pontuação significa que ele também é o mais rápido e confiável.
- **✓** Fazer a sua própria avaliação: tarefas reais do trabalho, cronometrando as respostas e conferindo cada chamada de ferramenta.
- Escolher o modelo mais barato no lugar.
- Esperar o ranking do mês que vem.

**Por quê:** Uma pontuação mede as tarefas do benchmark dela em um ajuste; ela não cronometra respostas no seu trabalho nem confere as suas ferramentas. A sua própria avaliação, algumas dezenas de tarefas reais que você mesmo pontua, mede exatamente isso. Escolher só pelo preço, ou esperar um ranking mais novo, deixa a mesma lacuna.

## O que você já sabe fazer

- Separar os limites rígidos que removem um modelo das qualidades que você pesa.
- Calcular um preço por tarefa e por mês a partir de tokens, turnos e preços de tabela.
- Ordenar uma lista curta pelo que importa para um agente e dizer do que a escolha abre mão.
- Apontar o que a página de um modelo não consegue dizer e testar isso na sua própria avaliação.

---

[Voltar para Entendendo os modelos](README.md)
