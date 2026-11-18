# Dépenser du calcul

Agents avancés · Chapitre 8 sur 10 · 20 minutes

Les boucles évaluateur-optimiseur, la réflexion, best-of-n et la recherche arborescente dépensent plus d'appels au modèle pour obtenir une meilleure réponse, et chacune a besoin d'un budget.

## Objectifs

- Exécuter une boucle générateur et juge avec une grille et un plafond de tours.
- Générer plusieurs réponses et en choisir une par vote.
- Explorer un petit arbre de plans avec un budget d'expansions.
- Réussir la vérification.

## La lecture

### Plus d'appels pour une meilleure réponse

Un appel au modèle donne une réponse. Plus d'appels peuvent en donner une meilleure : un juge note des brouillons, plusieurs échantillons sont comparés, ou une recherche regarde en avant. Chaque technique multiplie les appels, le coût et l'attente, donc chacun a besoin d'un budget qui l'arrête.

### Un générateur et un juge

Une boucle évaluateur-optimiseur a deux rôles. Le générateur écrit un brouillon. Le juge est un appel au modèle séparé, avec son propre prompt : il note le brouillon selon une grille, une courte liste de critères notés chacun sur une échelle, et écrit des notes pour le brouillon suivant. Un seuil dit quand un brouillon est validé, par exemple chaque critère à 4 ou plus sur 5. Un plafond de tours borne la boucle : quand il est atteint, la boucle renvoie le meilleur brouillon jusque-là, marqué comme non validé.

La réflexion, ou Self-Refine, est la même boucle où le modèle critique son propre brouillon. Un juge séparé est plus facile à garder strict et à tester. Comme dans Systèmes d'agents, chapitre 8, comparez ses notes à celles de personnes avant de lui faire confiance.

### Best-of-n et vote

Best-of-n demande n réponses au même prompt, échantillonnées à une température supérieure à 0 pour qu'elles diffèrent, et en garde une : un juge choisit la meilleure, ou un vote garde la réponse finale la plus fréquente. Voter sur les réponses finales s'appelle l'autocohérence (self-consistency) ; cela convient aux réponses que l'on peut comparer exactement, comme un montant. Dans LangGraph, les échantillons sont n Sends dans un superstep : n appels qui tournent en même temps, donc vous n'attendez que le plus lent.

### Recherche arborescente

Une recherche arborescente planifie des étapes à l'avance. Elle développe une étape en étapes suivantes possibles, donne à chacune une estimation de valeur, à quel point elle semble prometteuse, et continue de développer l'étape ouverte la plus prometteuse. Language Agent Tree Search (LATS, 2023) le fait avec une recherche arborescente Monte Carlo, en utilisant le modèle pour proposer des étapes, estimer des valeurs et réfléchir aux échecs. Un budget d'expansions limite le nombre d'étapes développées. Le lab exécute une recherche meilleur-d'abord plus petite sur deux niveaux, avec des valeurs simulées.

### Quand cela ne vaut pas la peine

Les appels supplémentaires sont rentables quand une meilleure réponse compte et que quelque chose peut distinguer le meilleur du pire : une grille, une réponse exacte sur laquelle voter, une valeur à estimer. Si un seul appel est déjà validé, ou si rien ne peut vérifier la qualité, ils n'ajoutent que du coût et du temps. Mesurez-le : exécutez vos cas d'évaluation avec et sans la technique.

## Essayez : Trois façons de dépenser plus d'appels

Le ticket T-48213 de Halden, de trois façons : une boucle avec juge peaufine la réponse, un vote choisit le montant du remboursement et une recherche arborescente planifie les étapes. Chacune s'arrête à son budget.

https://agenticademy.org/fr/paths/advanced-agents/08-spending-compute

## Vérification

Que renvoie cette exécution, et après combien d'appels au modèle (un par brouillon, un par évaluation) ?

Code:

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

- Le brouillon 3, marqué comme validé, après 6 appels.
- **✓** Le brouillon 2, marqué comme non validé, après 6 appels.
- Le brouillon 3, marqué comme non validé, après 6 appels.
- Rien encore : un quatrième tour s'exécute, car aucun brouillon n'a été validé.

**Pourquoi:** Aucun tour n'est validé : le tour 1 a complete 2, le tour 2 a tone 3, le tour 3 a complete 3 et tone 3. Donc review renvoie la boucle en arrière jusqu'à ce que rounds.length atteigne CAP, après le tour 3 : trois brouillons et trois évaluations, 6 appels, et pas de quatrième tour. finish garde ensuite le meilleur total, le brouillon 2 avec 11 contre le brouillon 3 avec 10, car une révision peut empirer ; le dernier brouillon n'est pas toujours le meilleur. passed vaut false, donc l'appelant peut signaler le brouillon ou le confier à une personne.

## Ce que vous savez faire maintenant

- Construire une boucle évaluateur-optimiseur avec un juge séparé, une grille, un seuil de validation et un plafond de tours.
- Exécuter des échantillons best-of-n comme Sends dans un superstep et garder la réponse sur laquelle la plupart s'accordent.
- Expliquer comment une recherche arborescente développe, note et remonte des valeurs avec un budget d'expansions.
- Décider quand des appels supplémentaires valent leur coût, et le mesurer avec vos cas d'évaluation.

---

[Retour à Agents avancés](README.md) · [Suivant : Un harnais de deep agent](09-deep-agent-harness.md)
