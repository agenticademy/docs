# L'état à grande échelle

Agents avancés · Chapitre 1 sur 10 · 20 minutes

Comment LangGraph.js 1.5.1 fusionne les écritures sur une clé de l'état : champs StateSchema avec et sans reducer, ordre des écritures en parallèle, Overwrite et UntrackedValue.

## Objectifs

- Exécutez deux nœuds en parallèle qui écrivent la même clé sans reducer, et lisez l'erreur.
- Ajoutez un reducer et voyez comment les deux écritures se combinent, dans l'ordre.
- Utilisez Overwrite pour remplacer une valeur à reducer.
- Prédisez la fin d'un superstep.

## La lecture

### Un superstep, plusieurs écritures

LangGraph exécute un graphe en supersteps, comme dans Systèmes d'agents, chapitre 5. Chaque nœud dû s'exécute dans le même superstep, en parallèle, et renvoie une mise à jour : un objet avec les clés de l'état qu'il modifie. Quand tous ont fini, LangGraph applique les mises à jour et enregistre un checkpoint de l'état. Les nœuds d'un même superstep ne voient jamais les mises à jour des autres, donc ce qui compte, c'est comment deux écritures sur la même clé se combinent.

### StateSchema et reducers

Depuis LangGraph.js 1.1, vous pouvez déclarer l'état avec StateSchema, un champ par clé. Un champ donné comme un simple schéma, par exemple z.string() de Zod, garde la dernière valeur écrite et accepte une écriture par superstep. Deux écritures dessus dans le même superstep échouent avec InvalidUpdateError, même quand les valeurs sont égales, et aucun checkpoint n'est enregistré pour ce superstep.

Un champ ReducedValue a un reducer : une fonction (current, next) => value que LangGraph appelle pour chaque écriture, par exemple pour ajouter à une liste. L'ancienne API Annotation.Root déclare la même chose avec Annotation({ reducer, default }).

### Quelle écriture passe en premier

Un reducer reçoit les écritures d'un superstep une par une, dans l'ordre des chemins de tâche, pas dans l'ordre où les nœuds ont fini. Les chemins de tâche sont comparés comme des chaînes, et le chemin d'un nœud lancé par une arête se termine par son nom, donc ici reviews passe avant specs. Depuis 1.5.0, les exécutions en direct et les checkpointers utilisent ce même ordre, comme LangGraph pour Python.

### Overwrite et UntrackedValue

new Overwrite(value), ajouté en 1.2, contourne le reducer : la clé prend la valeur value, par exemple pour remplacer une longue liste par un résumé. Les autres écritures sur cette clé dans le même superstep sont ignorées, et deux écritures Overwrite sur une clé dans un même superstep échouent.

Un champ UntrackedValue garde une valeur pendant que le graphe s'exécute : les nœuds la lisent, mais aucun checkpoint ne l'enregistre, donc une exécution reprise depuis un checkpoint démarre sans elle. Utilisez-le pour des données volumineuses ou temporaires que vous pouvez récupérer à nouveau.

### Le lab

Halden, un magasin d'articles de plein air fictif, se demande s'il doit stocker une tente pour l'hiver. Deux nœuds chercheurs, reviews et specs, ajoutent chacun une note à notes dans le même superstep ; condense résume ensuite les notes. Vous exécutez ce graphe trois fois : sans reducer sur notes, avec un reducer qui ajoute en fin de liste, et avec condense qui renvoie un Overwrite.

## Essayez : Une frise de supersteps

Exécutez le même graphe trois fois : sans reducer, avec un reducer, puis avec Overwrite. La frise montre ce que chaque superstep a écrit et ce que son checkpoint a enregistré.

https://agenticademy.org/fr/paths/advanced-agents/01-state-at-scale

## Vérification

reviews et specs suivent tous les deux START, donc ils s'exécutent dans le même superstep. Que se passe-t-il quand il se termine ?

Code:

```
const State = new StateSchema({
  topic: z.string(),
  notes: new ReducedValue(z.array(z.string()).default(() => []), {
    reducer: (current, next) => current.concat(next),
  }),
});

new StateGraph(State)
  .addNode("reviews", async () => ({ notes: ["reviews"], topic: "Ridge 2" }))
  .addNode("specs", async () => ({ notes: ["specs"], topic: "Ridge 2" }))
  .addEdge(START, "reviews")
  .addEdge(START, "specs");
```

- notes vaut ["reviews", "specs"] et topic vaut "Ridge 2".
- **✓** L'exécution échoue avec InvalidUpdateError sur topic.
- L'exécution échoue avec InvalidUpdateError sur notes.
- topic garde l'écriture du nœud qui a fini en dernier.

**Pourquoi:** topic est un champ simple, il accepte donc une écriture par superstep, et il en a reçu deux, même si les deux valent "Ridge 2". notes a un reducer, donc ses deux écritures seraient fusionnées, dans l'ordre des chemins de tâche. Aucun champ ne garde l'écriture qui a fini en dernier.

## Ce que vous savez faire maintenant

- Déclarer l'état avec StateSchema, en choisissant pour chaque clé un champ simple, un ReducedValue ou un UntrackedValue.
- Prédire comment des écritures en parallèle sur une clé se combinent, dans quel ordre, et quand elles échouent.
- Utiliser Overwrite pour remplacer une valeur à reducer.

---

[Retour à Agents avancés](README.md) · [Suivant : Command et Send](02-command-and-send.md)
