# Des boucles qui s'arrêtent

Agents avancés · Chapitre 6 sur 10 · 20 minutes

Une boucle a besoin d'arrêts qui tiennent quoi que fasse le modèle : une limite de récursion, un budget d'appels, un détecteur d'appels répétés et une personne qui peut dire non.

## Objectifs

- Lancez un agent qui répète un appel d'outil jusqu'à ce que GraphRecursionError l'arrête.
- Armez un budget d'appels et un détecteur de répétitions, et voyez lequel se déclenche en premier.
- Faites une pause avant un outil risqué et refusez l'appel.
- Réussissez la vérification.

## La lecture

### Une boucle a besoin d'arrêts que le modèle ne choisit pas

Une boucle d'agent se termine quand le modèle répond sans demander d'outil. Un tour, c'est un appel au modèle plus les outils qu'il demande. Si le modèle continue de demander, par exemple pour vérifier un statut qui ne change jamais, chaque tour est un nouvel appel payant au modèle et rien de nouveau n'est appris. Vous ajoutez donc des arrêts qui tiennent quoi que fasse le modèle.

### Le plafond de LangGraph : recursionLimit

Dans une boucle comme celle-ci, chaque exécution d'un nœud est un superstep. recursionLimit, dans la config de l'exécution, plafonne les supersteps, 25 par défaut. Au-delà, LangGraph.js lève GraphRecursionError, avec le lc\_error\_code GRAPH\_RECURSION\_LIMIT. C'est un filet de sécurité : il compte des supersteps, pas des tours, il s'arrête tard et, sans checkpointer, l'état de l'exécution est perdu avec l'erreur.

### Des arrêts sous forme de middleware

Le middleware, ce sont des hooks que le createAgent de LangChain 1.6 exécute autour de l'appel au modèle ; les hooks beforeModel et afterModel deviennent chacun un nœud du graphe. modelCallLimitMiddleware est un budget en appels au modèle : runLimit compte une exécution, threadLimit un fil entier, et exitBehavior 'end' termine l'exécution avec un message. toolCallLimitMiddleware compte les appels à un outil ou à tous. Un budget en tokens ou en argent fonctionne de la même façon : compter, comparer, arrêter. Ce site recrée les deux, sous les noms modelCallLimit et toolCallLimit ; le lab utilise modelCallLimit.

Un détecteur de répétitions repère tôt une boucle bloquée : le même outil avec les mêmes arguments, threshold fois dans les derniers appels. Le repeatDetector utilisé ici est propre à ce site ; LangChain n'en fournit pas. Avec exitBehavior 'end', il termine l'exécution ; avec 'continue', il refuse l'appel et dit au modèle d'essayer autre chose. Ici, chaque hook écrit aussi un événement de trace, pour que vous voyiez quel arrêt s'est déclenché.

### Une personne comme arrêt

Certains appels devraient attendre un oui, comme envoyer de l'argent. interruptBefore met le graphe en pause avant un nœud, et un checkpointer garde l'état en pause. Le point d'arrêt s'arrête avant chaque exécution de ce nœud, donc votre code choisit les appels qui demandent une personne et les lui montre. Pour refuser, updateState répond à l'appel avec un ToolMessage comme si le nœud tools s'était exécuté ; un nouvel invoke avec une entrée null reprend l'exécution. Sous Node, humanInTheLoopMiddleware fait cela avec interrupt(), qui a besoin d'AsyncLocalStorage ; les navigateurs ne l'ont pas, donc cette page utilise le point d'arrêt.

## Essayez : Trois façons d'arrêter une boucle

Un agent scripté vérifie un remboursement qui reste en attente. Arrêtez-le avec recursionLimit, puis avec un budget et un détecteur de répétitions, puis avec une personne avant l'outil risqué.

https://agenticademy.org/fr/paths/advanced-agents/06-loops-that-stop

## Vérification

Le modèle demande check\_refund({ order: '4471' }) à chaque tour et ne répond jamais. Quel arrêt termine cette exécution ?

Code:

```
const agent = createAgent({
  model,
  tools: [checkRefund],
  middleware: [
    modelCallLimit({
      runLimit: 3,
      exitBehavior: 'end',
    }),
    repeatDetector({
      window: 6,
      threshold: 4,
      exitBehavior: 'end',
    }),
  ],
});
// recursionLimit: 25 (the default)
await agent.graph.invoke({
  messages: [task],
});
```

- GraphRecursionError, au superstep 25.
- Le détecteur de répétitions, au quatrième appel identique.
- **✓** Le budget d'appels, avant le quatrième appel au modèle.
- Aucun : une exécution ne se termine que quand le modèle répond.

**Pourquoi:** Le budget d'appels permet trois appels au modèle ; avant le quatrième, modelCallLimit termine l'exécution. Le détecteur de répétitions ne voit donc jamais de quatrième appel identique. Ici, chaque tour prend cinq supersteps, donc l'exécution se termine au superstep 16, bien avant les 25 de recursionLimit. Et un arrêt n'attend jamais le modèle : c'est tout l'intérêt d'un arrêt.

## Ce que vous savez faire maintenant

- Régler recursionLimit comme filet de sécurité et intercepter GraphRecursionError.
- Ajouter un budget d'appels et un détecteur de répétitions comme middleware, et lire dans son événement de trace lequel s'est déclenché.
- Faire une pause avant un outil risqué avec interruptBefore, et refuser l'appel avec updateState.

---

[Retour à Agents avancés](README.md) · [Suivant : Boucles de planification](07-planning-loops.md)
