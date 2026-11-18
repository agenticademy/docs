# Voyager dans le temps

Agents avancés · Chapitre 4 sur 10 · 20 minutes

Chaque superstep enregistre un checkpoint : vous pouvez donc lire l'historique d'une exécution, rejouer depuis une étape passée, modifier un état passé et bifurquer vers une seconde fin.

## Objectifs

- Exécutez un graphe de cinq nœuds sur un thread et lisez son historique de checkpoints.
- Revenez à l'étape 3 et rejouez à partir de là.
- Modifiez l'état à l'étape 3, bifurquez et comparez les deux fins.
- Remettez dans l'ordre les étapes d'une bifurcation.

## La lecture

### Un thread est une frise

Compilez un graphe avec un checkpointer, exécutez-le avec un thread\_id, et LangGraph enregistre un checkpoint de tout l'état après chaque superstep, comme dans le parcours Systèmes d'agents. Chaque checkpoint a son propre checkpoint\_id, un step et une source dans ses métadonnées (le step -1 est l'entrée), et un parentConfig qui désigne le checkpoint précédent. Reliés par leurs parents, les checkpoints d'un thread forment une frise, et une frise peut se ramifier.

### Lire le passé

getStateHistory({ configurable: { thread\_id } }) liste tous les checkpoints du thread, du plus récent au plus ancien. Chaque élément est un instantané de l'état : values, next (les nœuds qui doivent s'exécuter), config et metadata. getState avec un checkpoint\_id dans sa config en lit un. C'est revenir en arrière : vous regardez un état passé et ne changez rien.

### Rejouer et bifurquer

invoke(null, config), avec une config qui désigne un checkpoint passé, rejoue depuis lui. Les nœuds avant ce checkpoint ne s'exécutent pas de nouveau ; ceux d'après, si, donc un vrai modèle peut répondre autrement la seconde fois. Les nouveaux checkpoints poussent sur une nouvelle branche qui commence par une copie dont la source est fork. Les anciens restent.

updateState(config, values, asNode) change le passé sans l'effacer. Il écrit un nouveau checkpoint, de source update, au-dessus de celui que désigne config, comme si le nœud asNode avait renvoyé values, et renvoie la config du nouveau checkpoint. Lancez invoke(null, thatConfig) et le thread bifurque : la première fin reste dans l'historique et une seconde pousse à partir de votre modification. getState sans checkpoint\_id lit le checkpoint le plus récent, qui est maintenant la fin de la bifurcation.

### Durabilité

L'option durability d'une exécution décide quand les checkpoints sont écrits. "async", la valeur par défaut, enregistre chacun pendant que le superstep suivant s'exécute ; "sync" l'enregistre avant que le superstep suivant ne commence ; "exit" n'enregistre qu'à la fin de l'exécution. "exit" écrit le moins, mais ne laisse aucune étape passée où revenir, ni rien d'où reprendre si le processus plante en cours de route.

### Dans ce lab

La vérification de remboursement de Halden pour le ticket T-48213 exécute cinq nœuds à la suite : read\_ticket, classify, list\_charges, decide et draft\_reply. LangGraph.js 1.5.1 et son MemorySaver tournent dans cette page, et classify, decide et draft\_reply interrogent un modèle scripté. La version navigateur de LangGraph.js n'a pas AsyncLocalStorage, donc chaque nœud passe explicitement sa config au modèle.

## Essayez : Un rail de checkpoints

Trois étapes : exécutez le graphe et lisez ses checkpoints, revenez à l'étape 3 et rejouez, puis modifiez l'étape 3, bifurquez et comparez les deux fins.

https://agenticademy.org/fr/paths/advanced-agents/04-time-travel

## Vérification

Remettez dans l'ordre les étapes pour bifurquer une exécution depuis un checkpoint passé.

1. Exécuter le graphe avec un checkpointer et un thread\_id.
2. Lister les checkpoints du thread avec getStateHistory.
3. Choisir par son checkpoint\_id le checkpoint où revenir.
4. Appeler updateState avec la config de ce checkpoint et les nouvelles valeurs.
5. Appeler invoke(null, config) avec la config renvoyée par updateState.
6. Comparer la fin de la bifurcation avec la première, toujours dans l'historique.

**Pourquoi:** Les checkpoints n'existent qu'une fois qu'une exécution sur un thread les a enregistrés, et vous trouvez un checkpoint passé dans getStateHistory par son checkpoint\_id. updateState a besoin de la config de ce checkpoint et renvoie la config du nouveau checkpoint qu'il écrit ; invoke(null, …) avec cette config exécute la bifurcation. Rien n'est écrasé, donc la première fin est toujours là pour comparer.

## Ce que vous savez faire maintenant

- Lire les checkpoints d'un thread avec getStateHistory, et l'un d'eux avec getState et son checkpoint\_id.
- Rejouer une exécution depuis un checkpoint passé, et dire quels nœuds s'exécutent de nouveau.
- Modifier un état passé avec updateState, bifurquer le thread et comparer les deux fins.
- Choisir un mode de durabilité en sachant où chacun vous laisse revenir.

---

[Retour à Agents avancés](README.md) · [Suivant : Des boucles qui tiennent](05-loops-that-survive.md)
