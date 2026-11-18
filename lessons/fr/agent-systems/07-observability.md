# Observabilité : traces et exécutions

Systèmes d'agents et standards · Chapitre 7 sur 8 · 20 minutes

Enregistrer chaque appel au modèle, chaque appel d'outil et chaque étape sous forme d'arbre d'exécutions, et le lire pour trouver ce qui est lent ou cassé.

## Objectifs

- Trouvez l'exécution de modèle ou d'outil la plus lente de la trace.
- Trouvez l'exécution d'outil qui a échoué et son parent.
- Exportez les exécutions au format de LangSmith.
- Réussissez la vérification.

## La lecture

### Exécutions et traces

La réponse d'un agent cache de nombreuses étapes. Le traçage enregistre chacune comme une exécution (run) : un graphe, un nœud, un appel au modèle ou un appel d'outil, avec ses entrées, ses sorties, ses heures de début et de fin, et une éventuelle erreur. Les exécutions derrière une requête forment un arbre, appelé une trace.

LangChain.js et LangGraph.js tracent par des callbacks. Passez un tracer comme LangChainTracer dans les callbacks de la config, et chaque runnable lui signale son début et sa fin.

### Le format d'exécution de LangSmith

Chaque exécution a un run\_type, comme chain, llm ou tool, un id, un trace\_id partagé par tout l'arbre et un parent\_run\_id. Son dotted\_order est une clé construite à partir de l'heure de début et de l'id de chaque exécution, de la racine jusqu'à elle, si bien que trier selon elle dispose l'arbre dans l'ordre où les choses se sont passées. Les exécutions du modèle portent un usage\_metadata avec le nombre de tokens. LangGraph ajoute thread\_id, langgraph\_node et langgraph\_step aux métadonnées.

### Les supersteps et l'arbre d'exécutions

LangGraph exécute un graphe en supersteps. Dans chaque superstep, les nœuds prévus s'exécutent, et leurs mises à jour sont appliquées ensemble avant que le superstep suivant commence. langgraph\_step est le numéro de ce superstep, donc une trace se lit de deux façons : comme un arbre d'exécutions, chaque exécution sous son parent, et comme une chronologie de supersteps, un groupe d'exécutions par étape. La cascade du lab groupe ses lignes par superstep.

### Ce qu'il faut chercher

La latence : quelle exécution est longue, et est-ce le modèle ou un outil. Les erreurs : une exécution d'outil qui a échoué et le nœud qui l'a appelée. Les tokens : quels appels lisent le plus. Les boucles : la même étape encore et encore.

Les traces contiennent des prompts, des résultats d'outils et des données clients, donc traitez-les comme sensibles. Le SDK LangSmith peut passer chaque exécution dans un anonymiseur avant de l'envoyer. OpenTelemetry est le standard général pour les traces entre services ; LangSmith accepte aussi les traces OpenTelemetry.

## Essayez : La cascade d'exécutions

Cinq étapes : lancez l'agent et regardez sa trace grandir, lisez ses supersteps, trouvez l'exécution la plus lente et l'outil qui a échoué, puis exportez les exécutions.

https://agenticademy.org/fr/paths/agent-systems/07-observability

## Vérification

### Question 1 sur 5

Que vous donne dotted\_order ?

- Le coût d'une exécution.
- **✓** Une clé triable qui place chaque exécution dans l'arbre de sa trace, dans l'ordre chronologique.
- Le nom du modèle.

**Pourquoi:** Elle est construite à partir de l'heure de début et de l'id de chaque exécution, de la racine jusqu'à celle-ci, si bien que trier les exécutions selon elle reconstruit l'arbre dans l'ordre où les choses se sont passées, sans suivre les liens vers les parents un par un.

### Question 2 sur 5

Deux exécutions d'une trace ont le même langgraph\_step. Qu'est-ce que cela vous dit ?

- Elles ont été appelées par le même outil.
- **✓** Elles se sont exécutées dans le même superstep du graphe.
- Elles ont pris le même temps.

**Pourquoi:** langgraph\_step est le numéro du superstep. Dans chaque superstep, LangGraph exécute les nœuds prévus et applique leurs mises à jour ensemble ; chaque exécution à l'intérieur de ces nœuds porte ce numéro.

### Question 3 sur 5

Qu'est-ce qu'une trace ?

- Un fichier journal d'erreurs.
- **✓** L'arbre des exécutions, modèle, outil et chaîne, derrière une requête.
- Une capture d'écran de l'app.

**Pourquoi:** Une requête devient une trace ; chaque étape à l'intérieur est une exécution avec son parent.

### Question 4 sur 5

Une exécution d'outil a échoué, mais l'agent a quand même répondu. Pourquoi l'exécution parente n'est-elle pas marquée comme échouée ?

- Le tracer cache les erreurs.
- **✓** Le nœud tools a intercepté l'erreur et l'a renvoyée au modèle comme résultat, donc le nœud lui-même s'est terminé.
- Les parents n'échouent jamais.

**Pourquoi:** Une erreur est enregistrée sur l'exécution qui l'a levée. Ici, le nœud l'a transformée en ToolMessage d'erreur, le modèle l'a lu et a réessayé.

### Question 5 sur 5

Qu'est-ce que vous devriez éviter d'envoyer à un service de traçage partagé ?

- Les durées des exécutions.
- **✓** Les données personnelles des clients dans les prompts et les résultats d'outils, sauf si vous en avez besoin et en avez le droit.
- Les types d'exécution.

**Pourquoi:** Les traces enregistrent les entrées et les sorties en entier. Masquez ou limitez ce qui est capturé avant que cela quitte votre système, par exemple avec l'anonymiseur du SDK.

## Ce que vous savez faire maintenant

- Lire une chronologie de supersteps et un arbre d'exécutions.
- Trouver l'exécution lente ou en échec dans une trace, et l'exécution qui l'a appelée.
- Nommer les principaux champs du format d'exécution de LangSmith.
- Masquer les données personnelles avant qu'une trace quitte votre système.

---

[Retour à Systèmes d'agents et standards](README.md) · [Suivant : Évaluation et sécurité](08-evaluation-and-safety.md)
