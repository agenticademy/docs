# Sous-graphes et équipes

Agents avancés · Chapitre 3 sur 10 · 20 minutes

Construisez des équipes d'agents à partir de sous-graphes, décidez comment elles partagent l'état, lisez leurs espaces de noms dans le flux et comparez des patterns multi-agents par appels au modèle et tokens.

## Objectifs

- Exécutez une équipe d'équipes et lisez les espaces de noms de son flux.
- Mettez en pause dans une équipe et ouvrez son propre état.
- Comparez sous-agents et passages de relais en appels au modèle et en tokens.
- Prédisez ce qu'un sous-graphe émet dans le flux.

## La lecture

### Sous-graphes

Un graphe compilé peut être un nœud d'un autre graphe : un sous-graphe. Quand les deux partagent une clé d'état, le sous-graphe compilé est le nœud, comme dans addNode("research\_team", researchTeam). Il lit les clés qu'il partage avec le parent, ce qu'il y écrit revient, et les clés qu'il est seul à avoir restent à l'intérieur.

Sans clé partagée, appelez le sous-graphe depuis un nœud enveloppe qui convertit l'état du parent en entrée du sous-graphe, et sa sortie en mise à jour. Passez { subgraphs: [team] } à addNode pour que getGraphAsync et getState puissent toujours le trouver.

### Espaces de noms et streaming

Chaque exécution d'un sous-graphe enregistre ses checkpoints dans son propre espace de noms à l'intérieur du thread du parent, nommé d'après son nœud et sa tâche : research\_team:<task id>. stream(input, { streamMode: "updates", subgraphs: true }) produit des paires [namespace, update], avec [] pour le graphe de premier niveau. getGraphAsync({ xray: true }) dessine les nœuds propres aux sous-graphes, comme research\_team:lead.

Pendant qu'une exécution est en pause dans un sous-graphe, getState(config, { subgraphs: true }) renvoie aussi l'état propre du sous-graphe. Dans un navigateur, interrupt() ne peut pas s'exécuter, car il a besoin d'AsyncLocalStorage ; ce labo met donc en pause avec interruptBefore et passe config à chaque sous-graphe à la main.

### Équipes d'équipes et passages de relais

Un superviseur peut diriger d'autres superviseurs. Dans le labo, une responsable du support oriente avec Command({ goto }) vers deux équipes, et le responsable de chaque équipe confie des parties du travail à ses workers. Un nœud dans un sous-graphe peut aussi passer la main à un nœud du parent : new Command({ graph: Command.PARENT, goto: "writer", update }). C'est un passage de relais : l'agent suivant prend la main avec l'historique que vous avez mis dans update.

### Choisir un pattern

La documentation JS de LangChain nomme cinq patterns multi-agents. Sous-agents : un agent principal appelle d'autres agents comme des outils et récupère chaque résultat. Passages de relais : l'agent aux commandes change. Skills : un agent charge des prompts de spécialiste quand il en a besoin. Router : une étape classe l'entrée et l'envoie à des agents. Workflow personnalisé : votre propre graphe LangGraph. Le chapitre 5 de Systèmes d'agents appelait passages de relais ses appels transfer\_to\_<agent>, mais son superviseur utilisait des sous-agents : chaque résultat lui revenait.

La documentation les compare (consultée le 2026-11-04) : une requête en un coup prend 4 appels au modèle avec des sous-agents et 3 avec des passages de relais ; une question sur trois domaines prend 5 appels et environ 9K tokens avec des sous-agents, 7+ appels et environ 14K+ avec des passages de relais. Dans le labo, vous exécutez un même travail des deux façons et vous comptez.

## Essayez : Une équipe d'équipes

Trois étapes : exécutez une équipe d'équipes et lisez son flux, mettez en pause dans une équipe pour ouvrir son état, puis exécutez le même travail en sous-agents et en passages de relais.

https://agenticademy.org/fr/paths/advanced-agents/03-subgraphs-and-teams

## Vérification

L'équipe partage messages avec le graphe de premier niveau ; notes est sa propre clé. Qu'affiche la boucle ?

Code:

```
const Team = Annotation.Root({
  ...MessagesAnnotation.spec,
  notes: Annotation({ reducer: (a, b) => a.concat(b), default: () => [] }),
});
const team = new StateGraph(Team)
  .addNode("work", () => ({ notes: ["n1"], messages: [{ role: "assistant", content: "done" }] }))
  .addEdge(START, "work")
  .compile();
const top = new StateGraph(MessagesAnnotation)
  .addNode("team", team)
  .addEdge(START, "team")
  .compile();

const input = { messages: [{ role: "user", content: "go" }] };
for await (const [ns, update] of await top.stream(input, { streamMode: "updates", subgraphs: true })) {
  const [node, values] = Object.entries(update)[0];
  console.log(ns.length ? "inside" : "top", node, Object.keys(values));
}
```

- **✓** inside work ["notes", "messages"], puis top team ["messages"] : d'abord la mise à jour propre de l'équipe, puis ce qui revient au graphe de premier niveau.
- top team ["notes", "messages"] : le flux ne montre que le graphe de premier niveau, et les clés du sous-graphe reviennent avec lui.
- inside work ["notes", "messages"], puis top team ["notes", "messages"] : chaque clé écrite par l'équipe revient.
- Une erreur est levée : le graphe de premier niveau n'a pas de clé notes, donc l'équipe ne peut pas en écrire une.

**Pourquoi:** Avec subgraphs: true, le flux produit aussi les mises à jour propres du sous-graphe, sous son espace de noms ["team:<task id>"] ; sans cela, vous ne verriez que la ligne top. L'équipe est le nœud lui-même, donc les clés qu'elle partage reviennent : messages. notes n'existe que dans l'équipe, donc elle y reste, et l'y écrire est permis.

## Ce que vous savez faire maintenant

- Ajouter un sous-graphe comme nœud quand il partage des clés, ou l'appeler depuis un nœud enveloppe qui convertit l'état.
- Streamer avec subgraphs: true et lire l'espace de noms de chaque mise à jour ; lire l'état d'un sous-graphe en pause avec getState.
- Passer la main à un nœud du graphe parent avec Command.PARENT, et construire des superviseurs de superviseurs.
- Choisir entre sous-agents, passages de relais, skills, router et workflow personnalisé selon leurs appels au modèle et leurs tokens.

---

[Retour à Agents avancés](README.md) · [Suivant : Voyager dans le temps](04-time-travel.md)
