# Orchestration : superviseurs, fan-out, sous-graphes

Systèmes d'agents et standards · Chapitre 5 sur 8 · 25 minutes

Des patterns pour plusieurs agents dans un même graphe LangGraph : un superviseur qui oriente, Send pour le travail en parallèle, les sous-graphes, et le checkpoint que chaque superstep enregistre.

## Objectifs

- Voyez deux spécialistes s'exécuter dans le même superstep.
- Ouvrez l'exécution d'un sous-graphe imbriqué.
- Exécutez la même tâche avec un seul agent et avec un superviseur.
- Prédisez le fan-out.

## La lecture

### Les supersteps

LangGraph exécute un graphe en supersteps. Dans chaque superstep, tous les nœuds prévus s'exécutent, en parallèle ; leurs mises à jour sont ensuite fusionnées dans l'état par le reducer de chaque clé, et les arêtes décident quels nœuds s'exécutent ensuite. Les messages utilisent un reducer qui ajoute à la fin, donc deux nœuds peuvent tous les deux ajouter des messages dans un même superstep.

### Threads et checkpoints

Compilez un graphe avec un checkpointer, comme MemorySaver, et passez un thread\_id quand vous l'exécutez. LangGraph enregistre un checkpoint de tout l'état sous ce thread : un pour l'entrée, puis un à la fin de chaque superstep. getState({ configurable: { thread\_id } }) lit le plus récent : les valeurs, le numéro d'étape et next, les nœuds qui doivent s'exécuter. Une exécution suivante sur le même thread repart de cet état.

### Superviseur, fan-out, sous-graphes

Un superviseur est un agent qui décide quel spécialiste travaille ensuite. Ici, un handoff est un appel d'outil comme transfer\_to\_customer\_support, donc le modèle le choisit comme n'importe quel autre outil.

Le fan-out exécute du travail en parallèle. Une arête conditionnelle peut renvoyer une liste d'objets Send au lieu de noms de nœuds. Chaque Send lance une exécution d'un nœud avec sa propre entrée, toutes dans le superstep suivant, et le reducer fusionne ce qu'elles renvoient.

Un sous-graphe est un graphe compilé utilisé dans un nœud. Il a son propre état et son propre espace de noms de checkpoints, et ses exécutions s'imbriquent sous le nœud qui l'a appelé. Dans ce lab, le Financial Analyst appelle son propre sous-graphe Data Engineer. Les handoffs avec Command et les sous-graphes qui partagent l'état viennent plus tard, dans le parcours Agents avancés.

### Quand un seul agent suffit

Chaque agent ajoute des appels au modèle, des checkpoints et des handoffs, et un endroit de plus où échouer. Découpez le travail quand ses parties sont indépendantes, quand un seul contexte deviendrait trop grand, ou quand une partie a besoin d'autres outils ou d'autres instructions. Sinon, commencez avec un seul agent.

Dans ce lab, l'équipe de support de Halden traite le ticket T-48213, un client débité deux fois pour la commande 48213. Exécutez-le des deux façons et comparez ce que mesurent les traces.

## Essayez : Un ciel d'agents

Trois étapes : exécutez la tâche avec un superviseur et regardez chaque superstep enregistrer un checkpoint, ouvrez un sous-graphe dans l'arbre des exécutions, puis exécutez la même tâche avec un seul agent et comparez.

https://agenticademy.org/fr/paths/agent-systems/05-orchestration

## Vérification

results a un reducer qui concatène les listes. Que renvoie le graphe ?

Code:

```
new StateGraph(State)
  .addNode("lookup", s => ({ results: [s.id * 10] }))
  .addConditionalEdges(START, () => [new Send("lookup", { id: 1 }), new Send("lookup", { id: 2 })], ["lookup"])
  .addEdge("lookup", END)
  .compile()
  .invoke({});
```

- **✓** { results: [10, 20] } : deux exécutions de lookup dans un même superstep.
- { results: [20] } : le second Send écrase le premier.
- { results: [10] } : seul le premier Send s'exécute.
- Il lève une erreur : on ne peut pas envoyer deux fois vers un nœud.

**Pourquoi:** Chaque Send lance sa propre exécution de lookup avec sa propre entrée ({ id: 1 } et { id: 2 }). Les deux s'exécutent dans le même superstep, et le reducer concatène leurs mises à jour.

## Ce que vous savez faire maintenant

- Construire un superviseur qui oriente vers des agents spécialistes avec des appels d'outils de handoff.
- Faire un fan-out avec Send et fusionner les résultats avec un reducer.
- Utiliser un graphe compilé comme sous-graphe, et dire quand un seul agent suffit.
- Exécuter un graphe sur un thread et lire son dernier checkpoint avec getState.

---

[Retour à Systèmes d'agents et standards](README.md) · [Suivant : Ingénierie du contexte et récupération](06-context-engineering.md)
