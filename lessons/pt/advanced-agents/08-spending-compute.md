# Gastar computação

Agentes avançados · Capítulo 8 de 10 · 20 minutos

Loops avaliador-otimizador, reflexão, best-of-n e busca em árvore gastam mais chamadas ao modelo para chegar a uma resposta melhor, e cada um precisa de um orçamento.

## Objetivos

- Rode um loop de gerador e juiz com uma rubrica e um limite de rodadas.
- Gere várias respostas e escolha uma por votação.
- Busque em uma pequena árvore de planos com um orçamento de expansões.
- Passe na verificação.

## A leitura

### Mais chamadas para uma resposta melhor

Uma chamada ao modelo dá uma resposta. Mais chamadas podem dar uma melhor: um juiz avalia rascunhos, várias amostras são comparadas, ou uma busca olha à frente. Cada padrão multiplica as chamadas, o custo e a espera, então cada um precisa de um orçamento que o pare.

### Um gerador e um juiz

Um loop avaliador-otimizador tem dois papéis. O gerador escreve um rascunho. O juiz é uma chamada ao modelo separada, com seu próprio prompt: ele pontua o rascunho segundo uma rubrica, uma lista curta de critérios pontuados cada um em uma escala, e escreve notas para o próximo rascunho. Um limiar diz quando um rascunho passa, como cada critério com 4 ou mais de 5. Um limite de rodadas restringe o loop: quando é atingido, o loop retorna o melhor rascunho até ali, marcado como não aprovado.

A reflexão, ou Self-Refine, é o mesmo loop com o modelo criticando o próprio rascunho. Um juiz separado é mais fácil de manter rigoroso e de testar. Como em Sistemas de agentes, capítulo 8, compare as pontuações dele com as de pessoas antes de confiar nele.

### Best-of-n e votação

Best-of-n pede n respostas ao mesmo prompt, amostradas com temperatura acima de 0 para que sejam diferentes, e fica com uma: um juiz escolhe a melhor, ou uma votação fica com a resposta final mais comum. Votar nas respostas finais se chama autoconsistência (self-consistency); serve para respostas que você pode comparar com exatidão, como um valor em dinheiro. No LangGraph, as amostras são n Sends em um superstep: n chamadas que rodam ao mesmo tempo, então você espera só pela mais lenta.

### Busca em árvore

Uma busca em árvore planeja passos à frente. Ela expande um passo em possíveis próximos passos, dá a cada um uma estimativa de valor, o quanto ele parece promissor, e continua expandindo o passo aberto mais promissor. A Language Agent Tree Search (LATS, 2023) faz isso com busca em árvore de Monte Carlo, usando o modelo para propor passos, estimar valores e refletir sobre falhas. Um orçamento de expansões limita quantos passos ela expande. O laboratório roda uma busca menor, melhor-primeiro, em dois níveis, com valores simulados.

### Quando não vale a pena

Chamadas extras compensam quando uma resposta melhor importa e algo consegue distinguir o melhor do pior: uma rubrica, uma resposta exata para votar, um valor para estimar. Se uma chamada já passa, ou nada consegue verificar a qualidade, elas só somam custo e tempo. Meça: rode seus casos de avaliação com e sem o padrão.

## Experimente: Três formas de gastar mais chamadas

O ticket T-48213 da Halden, de três formas: um loop com juiz refina a resposta, uma votação escolhe o valor do reembolso e uma busca em árvore planeja os passos. Cada uma para no seu orçamento.

https://agenticademy.org/pt/paths/advanced-agents/08-spending-compute

## Verificação

O que esta execução retorna, e depois de quantas chamadas ao modelo (uma por rascunho, uma por avaliação)?

Código:

```
const CAP = 3;
const PASS = 4; // per criterion, of 5
const scores = (r) =>
  Object.values(r.scores);
const passes = (r) =>
  scores(r).every((s) => s >= PASS);
const total = (r) =>
  scores(r).reduce((a, b) => a + b);

function review(state) {
  const last = state.rounds.at(-1);
  if (passes(last)) return 'finish';
  if (state.rounds.length >= CAP)
    return 'finish';
  return 'generate';
}

function finish(state) {
  const last = state.rounds.at(-1);
  const best = passes(last) ? last
    : state.rounds.reduce((b, r) =>
        (total(r) > total(b) ? r : b));
  return {
    answer: best.draft,
    passed: passes(best),
  };
}

// The judge's scores, round by round:
// 1: accurate 3, complete 2, tone 4
// 2: accurate 4, complete 4, tone 3
// 3: accurate 4, complete 3, tone 3
```

- O rascunho 3, marcado como aprovado, depois de 6 chamadas.
- **✓** O rascunho 2, marcado como não aprovado, depois de 6 chamadas.
- O rascunho 3, marcado como não aprovado, depois de 6 chamadas.
- Nada ainda: roda uma quarta rodada, porque nenhum rascunho passou.

**Por quê:** Nenhuma rodada passa: a rodada 1 tem complete 2, a rodada 2 tem tone 3, a rodada 3 tem complete 3 e tone 3. Então review manda o loop de volta até rounds.length chegar a CAP, depois da rodada 3: três rascunhos e três avaliações, 6 chamadas, e nenhuma quarta rodada. Depois finish fica com o melhor total, o rascunho 2 com 11 contra o rascunho 3 com 10, porque uma revisão pode piorar; o último rascunho nem sempre é o melhor. passed é false, então quem chamou pode marcar o rascunho ou passá-lo para uma pessoa.

## O que você já sabe fazer

- Construir um loop avaliador-otimizador com um juiz separado, uma rubrica, um limiar de aprovação e um limite de rodadas.
- Rodar amostras de best-of-n como Sends em um superstep e ficar com a resposta em que a maioria concorda.
- Explicar como uma busca em árvore expande, pontua e propaga valores com um orçamento de expansões.
- Decidir quando chamadas extras valem o custo, e medir isso com seus casos de avaliação.

---

[Voltar para Agentes avançados](README.md) · [Próximo: Um harness de deep agent](09-deep-agent-harness.md)
