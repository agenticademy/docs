# Votre premier graphe

Construisez vos premiers agents · Chapitre 4 sur 8 · 20 minutes

Construire un StateGraph avec des nœuds et des arêtes, router avec une arête conditionnelle et l'exécuter pas à pas.

## Objectifs

- Reliez START, classify, les deux guichets et END en un graphe qui compile.
- Exécutez le graphe de sorte que chaque branche soit prise une fois.
- Prédisez comment le graphe s'exécute.

## La lecture

### Nœuds et arêtes

Un StateGraph part d'une définition d'état, comme Annotation.Root({ … }) ou la MessagesAnnotation du chapitre 3. addNode(name, fn) ajoute un nœud : une fonction qui lit l'état et renvoie une mise à jour, que les reducers de l'état fusionnent. addEdge(a, b) indique que b s'exécute après a. START et END sont l'entrée et la sortie du graphe : une arête depuis START choisit le premier nœud, et une arête vers END termine l'exécution.

### Branches

addConditionalEdges(from, router, targets) place une décision après un nœud. Le routeur est une fonction de l'état qui renvoie le nom du nœud suivant, et targets liste les nœuds qu'il peut renvoyer. C'est ainsi qu'un graphe envoie une demande de remboursement vers un guichet et une question vers un autre. Sans routeur, deux arêtes simples sortant d'un même nœud s'exécutent toutes les deux.

### Compiler, puis exécuter

compile() vérifie la structure et renvoie un graphe que vous pouvez appeler avec invoke() ou stream(). Elle lève une erreur si quelque chose ne va pas, par exemple un nœud qu'aucune arête n'atteint (UnreachableNodeError). Elle n'exige pas que chaque nœud mène à END : un nœud sans arête sortante termine simplement l'exécution à cet endroit.

Une exécution avance par supersteps. À chacun, tous les nœuds prêts s'exécutent ensemble ; puis leurs mises à jour sont fusionnées dans l'état, et les arêtes décident quels nœuds sont prêts ensuite. stream() signale la mise à jour de chaque nœud au moment où elle se produit. Si deux nœuds d'un même superstep écrivent la même clé, cette clé a besoin d'un reducer pour les combiner, sinon LangGraph lève une erreur.

## Essayez : Câblez un graphe

Tracez les cinq arêtes de START à END ; LangGraph compile le graphe après chacune. Puis exécutez les deux tickets d'exemple et suivez chaque superstep.

https://agenticademy.org/fr/paths/first-agents/04-your-first-graph

## Vérification

Comment ce graphe s'exécute-t-il ?

Code:

```
new StateGraph(State)
  .addNode("a", a).addNode("b", b).addNode("c", c)
  .addEdge(START, "a")
  .addEdge("a", "b").addEdge("a", "c")
  .addEdge("b", END).addEdge("c", END)
  .compile();
```

- a, puis b, puis c : trois supersteps.
- **✓** a, puis b et c ensemble dans un superstep : deux supersteps.
- a, b et c en même temps.
- Il ne compile pas : un nœud ne peut pas avoir deux arêtes sortantes.

**Pourquoi:** Une fois a terminé, b et c sont tous deux prêts : ils s'exécutent dans le même superstep et leurs mises à jour fusionnent.

## Ce que vous savez faire maintenant

- Construire et compiler un StateGraph avec des nœuds, des arêtes, START et END.
- Router avec addConditionalEdges.
- Expliquer les supersteps et quand les nœuds s'exécutent en parallèle.

---

[Retour à Construisez vos premiers agents](README.md) · [Suivant : Outils et appels d'outils](05-tools-and-tool-calls.md)
