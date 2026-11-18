# La boucle de l'agent : un agent ReAct minimal

Construisez vos premiers agents · Chapitre 6 sur 8 · 20 minutes

Un nœud modèle, un nœud outils et une arête conditionnelle : toute la boucle de l'agent sous forme de graphe LangGraph.

## Objectifs

- Avancez l'agent superstep par superstep jusqu'à ce qu'il réponde.
- Comptez combien de fois le modèle s'est exécuté.
- Remettez l'exécution dans l'ordre.

## La lecture

### Raisonner, agir, observer, recommencer

ReAct (reason and act, raisonner et agir) est un patron de conception pour les agents : le modèle lit la conversation et soit demande des outils, soit répond. Quand il en demande, votre code exécute les outils et ajoute leurs résultats à la conversation, puis le modèle relit. La boucle se termine quand le modèle répond sans demander d'outil.

### La boucle sous forme de graphe

Dans LangGraph, la boucle tient en deux nœuds sur MessagesAnnotation, l'état qui contient la liste des messages. Le nœud agent appelle le modèle, outils liés, sur state.messages et renvoie sa réponse. Le nœud tools est ToolNode, de @langchain/langgraph/prebuilt : il exécute chaque appel d'outil du dernier AIMessage et ajoute un ToolMessage par appel. Les arguments invalides deviennent des ToolMessages d'erreur, comme au chapitre 5.

toolsCondition est le routeur placé après le nœud agent : il renvoie "tools" quand le dernier message a des tool\_calls et END quand il n'en a pas. Une arête ramène de tools à agent. C'est tout l'agent, environ 25 lignes.

LangGraph exécute un graphe en supersteps : à chacun, les nœuds prévus s'exécutent une fois. Dans ce graphe, un seul nœud est prévu à la fois, donc chaque superstep est l'exécution d'un nœud, et graph.stream(input, { streamMode: "updates" }) produit une mise à jour par superstep. Vous pouvez regarder la boucle tourner.

### Prêt à l'emploi, et plafonné

createReactAgent, dans @langchain/langgraph/prebuilt, construit le même graphe ; il est déprécié au profit de createAgent dans le paquet langchain. Construisez-le une fois à la main et vous saurez ce que font ces aides.

Une boucle peut tourner sans fin si le modèle continue à demander des outils. recursionLimit, dans la config de l'exécution, plafonne les supersteps (25 par défaut) ; quand la limite est atteinte, LangGraph lève GraphRecursionError. Essayez une limite de 4 ci-dessous : cette exécution a besoin de 5 supersteps.

## Essayez : La boucle, une étape à la fois

Trois étapes, une action chacune, et une quatrième facultative : envoyez le ticket T-48213, avancez la boucle jusqu'à ce que l'agent réponde, comptez les appels au modèle, puis plafonnez la boucle avec recursionLimit.

https://agenticademy.org/fr/paths/first-agents/06-the-agent-loop

## Vérification

Remettez dans l'ordre l'exécution du ticket T-48213.

1. Le nœud agent envoie la tâche au modèle.
2. Le modèle demande get\_ticket.
3. ToolNode exécute get\_ticket et ajoute un ToolMessage.
4. Le modèle le lit et demande list\_charges.
5. ToolNode exécute list\_charges et ajoute un ToolMessage.
6. Le modèle répond sans appel d'outil.
7. toolsCondition route vers END.

**Pourquoi:** À chaque tour, le modèle soit demande des outils, ce qui boucle par ToolNode, soit répond, ce qui termine l'exécution. Le modèle s'est exécuté trois fois.

## Ce que vous savez faire maintenant

- Construire un agent ReAct sous forme de graphe avec ToolNode et toolsCondition.
- Suivre une exécution message par message avec streamMode "updates".
- Expliquer pourquoi toute boucle d'agent a besoin d'une limite de récursion.

---

[Retour à Construisez vos premiers agents](README.md) · [Suivant : Callbacks et approbation humaine](07-callbacks-and-approval.md)
