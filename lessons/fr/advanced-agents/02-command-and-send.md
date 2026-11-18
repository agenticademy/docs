# Command et Send

Agents avancés · Chapitre 2 sur 10 · 20 minutes

Un nœud peut mettre à jour l'état et choisir le nœud suivant en une seule étape avec Command, et répartir le travail entre des workers en parallèle avec Send, autant que le modèle en demande.

## Objectifs

- Exécutez un nœud routeur qui met à jour l'état et saute avec Command.
- Répartissez avec Send sur autant de vérifications que le modèle scripté en choisit, pour deux tickets.
- Reprenez l'exécution : le reducer a rassemblé les résultats, et un nœud les résume.
- Prédisez avec quoi s'exécute le nœud qui rassemble.

## La lecture

### Command : mettre à jour et router en un seul return

Un nœud renvoie d'habitude une mise à jour de l'état, et les arêtes du graphe décident de ce qui s'exécute ensuite. Un nœud peut à la place renvoyer new Command({ update, goto }). update est écrit dans l'état comme le serait un return ordinaire, et goto nomme le nœud suivant, choisi pendant que le nœud s'exécute. Un routeur peut classer un ticket, enregistrer la catégorie et sauter au bon nœud en une seule étape.

Déclarez où un Command peut aller avec ends : addNode("triage", triage, { ends: ["plan", "reply"] }). compile() utilise ends pour vérifier que chaque nœud peut être atteint : ici plan et reply n'ont pas d'autre entrée, donc sans ends il lève UnreachableNodeError. getGraphAsync() dessine ends comme des arêtes. Un goto s'ajoute aux arêtes statiques du nœud ; il ne les remplace pas, donc un nœud qui renvoie Command n'en a généralement aucune.

### Send : autant de workers que le modèle en demande

new Send("check", { ticket, check }) lance une exécution d'un nœud, appelée tâche, avec sa propre entrée au lieu de l'état du graphe. Une arête conditionnelle qui renvoie une liste de Sends répartit le travail : toutes ces tâches s'exécutent dans le superstep suivant, en même temps. La liste peut venir du modèle, donc la largeur se décide à l'exécution : trois vérifications pour un ticket, cinq pour un autre. Le goto de Command accepte aussi des Sends.

### Rassembler les résultats

Les tâches écrivent toutes la même clé, donc cette clé a besoin d'un reducer, comme au chapitre précédent. À la fin du superstep, le reducer fusionne leurs écritures dans l'ordre des tâches, qui est l'ordre des Sends, quel que soit l'ordre dans lequel les tâches ont fini. Un nœud qui a une arête depuis les workers s'exécute alors une fois, dans le superstep suivant, et les lit toutes. C'est du map-reduce dans un graphe.

### Passages de relais, et ce labo

Dans un sous-graphe, new Command({ graph: Command.PARENT, goto }) saute vers un nœud du graphe parent. C'est ainsi qu'un agent passe du travail à un autre ; le chapitre suivant construit des équipes avec.

Dans le labo, le service des tickets de Halden, la boutique fictive du chapitre précédent, exécute un seul graphe : triage route chaque ticket, plan laisse le modèle scripté choisir les vérifications, et summarise écrit la réponse. La page met les exécutions en pause avec interruptAfter et interruptBefore, passés à stream(), parce que interrupt() a besoin d'AsyncLocalStorage, que la version navigateur de LangGraph.js n'a pas.

## Essayez : Router, répartir, rassembler

Trois étapes sur un même graphe LangGraph.js : routez un ticket avec Command, répartissez sur les vérifications que choisit le modèle, puis reprenez l'exécution pour rassembler et résumer.

https://agenticademy.org/fr/paths/advanced-agents/02-command-and-send

## Vérification

Le modèle scripté a choisi trois vérifications, et chaque vérification prend un temps différent. Combien de fois summarise s'exécute-t-il, et qu'écrit-il ?

Code:

```
// findings: a ReducedValue whose
// reducer concatenates lists.
// plan returns { checks:
//   ["order", "shipping", "stock"] }
new StateGraph(State)
  .addNode("plan", plan)
  .addNode("check", async ({ check }) =>
    ({ findings: [check] }))
  .addNode("summarise", (s) => ({
    answer: s.findings.join(" + "),
  }))
  .addEdge(START, "plan")
  .addConditionalEdges("plan", (s) =>
    s.checks.map((c) =>
      new Send("check", { check: c })),
    ["check"])
  .addEdge("check", "summarise")
  .addEdge("summarise", END)
  .compile();
```

- Trois fois, une par vérification terminée : d'abord un nom, puis deux, puis les trois.
- **✓** Une fois, après les trois : "order + shipping + stock", dans l'ordre des Sends.
- Une fois, avec les trois noms dans l'ordre où les vérifications ont fini, donc cela peut changer d'une exécution à l'autre.
- Une fois : "stock". L'écriture de chaque vérification remplace la précédente.

**Pourquoi:** Les trois Sends lancent trois tâches check dans le même superstep. Quand il se termine, le reducer fusionne leurs écritures dans l'ordre des tâches, qui est l'ordre des Sends, pas l'ordre où elles ont fini. summarise a une arête depuis check, donc il s'exécute une fois, dans le superstep suivant, et lit les trois. Sans le reducer, trois écritures dans findings au cours d'un même superstep échoueraient.

## Ce que vous savez faire maintenant

- Router et mettre à jour l'état en une seule étape en renvoyant Command depuis un nœud, avec ses cibles déclarées dans ends.
- Répartir avec Send sur une largeur que le modèle choisit à l'exécution.
- Rassembler des résultats parallèles avec un reducer, et prédire leur ordre et le moment où le nœud suivant s'exécute.

---

[Retour à Agents avancés](README.md) · [Suivant : Sous-graphes et équipes](03-subgraphs-and-teams.md)
