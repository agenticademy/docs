# Boucles de planification

Agents avancés · Chapitre 7 sur 10 · 20 minutes

Plan-and-execute, ReWOO et LLMCompiler planifient le travail avant de le faire, et diffèrent en appels au modèle, en supersteps et en temps.

## Objectifs

- Lancez plan-and-execute : un plan, une étape à la fois et une replanification après chaque étape.
- Lancez ReWOO : un plan avec des marqueurs #E, les outils, puis une réponse.
- Lancez LLMCompiler : un graphe de tâches dont les tâches indépendantes tournent en même temps.
- Réussissez la vérification.

## La lecture

### D'abord planifier, ensuite agir

Les agents vus jusqu'ici décidaient un appel d'outil à la fois : appeler le modèle, exécuter l'outil, rappeler le modèle. C'est la boucle ReAct. Un agent qui planifie demande d'abord un plan au modèle, puis le suit. Trois motifs publiés le font de façons différentes. Le lab les compare sur trois compteurs : les appels au modèle, les supersteps (une ronde d'exécutions de nœuds, comme au chapitre 1) et la durée d'une exécution.

### Plan-and-execute

Un nœud planner écrit une liste d'étapes. Un nœud agent fait la première : le modèle choisit un appel d'outil, et le nœud l'exécute. Un nœud replan lit ce qui est fait, puis répond ou réécrit les étapes restantes. Le plan peut changer après chaque résultat, mais chaque étape coûte au moins deux appels au modèle : un pour agir, un pour replanifier. L'idée vient de l'article Plan-and-Solve (2023) et du tutoriel plan-and-execute de LangGraph.

### ReWOO

ReWOO (2023) écrit tout le plan en un seul appel au modèle et nomme chaque résultat avant qu'il existe : #E1 = search[…], puis #E3 = forecast[#E1]. #E1 est un marqueur. Un nœud tool exécute les étapes dans l'ordre et met chaque résultat à la place de son nom, sans appel au modèle. Un nœud solve appelle le modèle une dernière fois pour répondre. Cela fait deux appels au modèle en tout, mais le plan ne peut plus changer une fois écrit.

### LLMCompiler

LLMCompiler (2023) écrit le plan comme un graphe de tâches sans boucles, un DAG : 3. forecast($1) signifie que la tâche 3 a besoin du résultat de la tâche 1. Un ordonnanceur, l'unité de récupération des tâches (task fetching unit) de l'article, lance chaque tâche dès que les tâches dont elle a besoin sont terminées, donc les tâches qui n'ont besoin de rien les unes des autres tournent en même temps. Un nœud join lit les résultats, puis répond ou demande un nouveau plan.

### Dans ce lab

Une tâche de recherche tourne de trois façons : la météo de samedi là où part chacune de deux courses de trail, pour Halden, la boutique fictive. Chaque graphe est du vrai LangGraph.js 1.5.1, dessiné à partir de getGraphAsync(). Dans Node, vous pourriez aussi écrire l'ordonnanceur avec l'API fonctionnelle, comme un entrypoint dont les appels à task() tournent en parallèle. task() a besoin d'AsyncLocalStorage, que la version pour navigateur n'a pas, donc ici le nœud schedule lance de simples promesses et transmet config à la main. Le modèle est scripté, et chaque appel prend un temps scripté, accéléré à l'écran et compté comme temps simulé.

## Essayez : Une tâche, trois planificateurs

Lancez la même tâche de recherche en plan-and-execute, ReWOO et LLMCompiler, puis comparez leurs appels au modèle, leurs supersteps et leur temps simulé.

https://agenticademy.org/fr/paths/advanced-agents/07-planning-loops

## Vérification

### Question 1 sur 3

Une tâche demande cinq recherches indépendantes les unes des autres, puis une réponse. Quel motif peut finir le plus tôt ?

- Plan-and-execute : replan tient le plan à jour.
- ReWOO : il n'appelle le modèle que deux fois.
- **✓** LLMCompiler : les cinq recherches peuvent tourner en même temps.
- Les trois prennent le même temps, car les recherches sont les mêmes.

**Pourquoi:** L'ordonnanceur de LLMCompiler lance chaque tâche dont les entrées sont prêtes, donc les cinq recherches se chevauchent et prennent à peu près le temps de la plus lente. ReWOO n'appelle lui aussi le modèle que deux fois, mais son nœud tool exécute une recherche par superstep, l'une après l'autre. Plan-and-execute ajoute deux appels au modèle par recherche. Les recherches sont les mêmes ; le moment où elles tournent ne l'est pas.

### Question 2 sur 3

Un plan ReWOO contient l'étape #E3 = forecast[#E1]. Que devient #E1 avant que forecast tourne ?

- **✓** Le nœud tool met le résultat de l'étape #E1 à sa place.
- Le modèle est rappelé pour transformer #E1 en ville.
- forecast reçoit le texte « #E1 » et le cherche.
- Le nœud solve le remplit à la fin.

**Pourquoi:** #E1 est un marqueur pour le résultat de l'étape nommée #E1. Le nœud tool le remplace par ce résultat avant d'appeler forecast, donc aucun appel au modèle n'est nécessaire entre les étapes. C'est ainsi que ReWOO s'en sort avec deux appels au modèle : un pour planifier, et un dans le nœud solve, qui tourne après tous les outils, trop tard pour remplir quoi que ce soit.

### Question 3 sur 3

La Lake Loop est annulée, donc la recherche de sa ville de départ renvoie « Aucun résultat. » Quel motif peut retirer la météo de la Lake Loop avant qu'elle tourne ?

- **✓** Plan-and-execute : replan lit chaque résultat avant l'étape suivante.
- ReWOO : le nœud tool saute une étape dont l'entrée n'a rien trouvé.
- LLMCompiler : l'ordonnanceur saute une tâche dont l'entrée n'a rien trouvé.
- Aucun : un plan ne peut plus changer une fois écrit.

**Pourquoi:** Plan-and-execute appelle replan après chaque étape, donc il peut retirer la météo des étapes restantes : c'est ce que paient ses appels au modèle en plus. Le nœud tool de ReWOO ne fait que remplir les résultats, donc il lance forecast[#E2] avec « Aucun résultat. » comme ville. L'ordonnanceur de LLMCompiler lance lui aussi forecast($2) comme prévu ; seul le nœud join, après toutes les tâches, peut demander un nouveau plan.

## Ce que vous savez faire maintenant

- Construire plan-and-execute, ReWOO et LLMCompiler comme graphes LangGraph.js.
- Compter ce que coûte chaque motif en appels au modèle, en supersteps et en temps, et dire d'où vient la différence.
- Choisir un motif : replanifier quand les résultats peuvent changer le plan, planifier une fois quand ils ne le peuvent pas, et lancer en même temps les étapes indépendantes.

---

[Retour à Agents avancés](README.md) · [Suivant : Dépenser du calcul](08-spending-compute.md)
