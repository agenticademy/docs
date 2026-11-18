# Agents avancés

Avancé · Pour les développeurs · 10 chapitres · 205 min

Pour les développeurs qui savent déjà construire des systèmes d'agents. Vous travaillez avec de grands graphes et leur état, vous routez avec Command et Send, vous rembobinez et bifurquez des exécutions, vous construisez des boucles qui se relèvent des échecs et savent s'arrêter, vous comparez des schémas de planification et vous faites tourner un harness de deep agents sur une longue tâche. Chaque lab exécute le vrai LangGraph.js 1.5.1 dans votre navigateur, avec un modèle scripté. Le harness de deep agents est recréé, et les écrans le disent.

## Avant de commencer

Le parcours Systèmes d'agents et standards, ou les mêmes connaissances : graphes, outils, boucle de l'agent, checkpoints et traces.

## À la fin, vous saurez

- Concevoir l'état d'un graphe avec des reducers et Overwrite, et prévoir comment se combinent des écritures parallèles.
- Router avec Command et Send, et former des équipes à partir de sous-graphes.
- Rembobiner, modifier et bifurquer une exécution à partir de ses checkpoints.
- Construire des boucles qui réessaient, compensent, reprennent après un plantage et s'arrêtent sur des limites, des budgets ou la décision d'une personne.
- Choisir un schéma de planification ou de dépense de calcul, et faire tourner un harness de deep agents sur une longue tâche.
- Mener une longue tâche sur plusieurs sessions avec des fichiers de passation, la compaction et un Store, et évaluer sa trajectoire avec des règles écrites en code.

## Chapitres

1. [L'état à grande échelle](01-state-at-scale.md): Comment LangGraph.js 1.5.1 fusionne les écritures sur une clé de l'état : champs StateSchema avec et sans reducer, ordre des écritures en parallèle, Overwrite et UntrackedValue.
2. [Command et Send](02-command-and-send.md): Un nœud peut mettre à jour l'état et choisir le nœud suivant en une seule étape avec Command, et répartir le travail entre des workers en parallèle avec Send, autant que le modèle en demande.
3. [Sous-graphes et équipes](03-subgraphs-and-teams.md): Construisez des équipes d'agents à partir de sous-graphes, décidez comment elles partagent l'état, lisez leurs espaces de noms dans le flux et comparez des patterns multi-agents par appels au modèle et tokens.
4. [Voyager dans le temps](04-time-travel.md): Chaque superstep enregistre un checkpoint : vous pouvez donc lire l'historique d'une exécution, rejouer depuis une étape passée, modifier un état passé et bifurquer vers une seconde fin.
5. [Des boucles qui tiennent](05-loops-that-survive.md): Les politiques de nouvelle tentative, les timeouts, les gestionnaires d'erreurs et les checkpoints maintiennent une longue exécution quand un outil échoue ou que le processus s'arrête.
6. [Des boucles qui s'arrêtent](06-loops-that-stop.md): Une boucle a besoin d'arrêts qui tiennent quoi que fasse le modèle : une limite de récursion, un budget d'appels, un détecteur d'appels répétés et une personne qui peut dire non.
7. [Boucles de planification](07-planning-loops.md): Plan-and-execute, ReWOO et LLMCompiler planifient le travail avant de le faire, et diffèrent en appels au modèle, en supersteps et en temps.
8. [Dépenser du calcul](08-spending-compute.md): Les boucles évaluateur-optimiseur, la réflexion, best-of-n et la recherche arborescente dépensent plus d'appels au modèle pour obtenir une meilleure réponse, et chacune a besoin d'un budget.
9. [Un harnais de deep agent](09-deep-agent-harness.md): Un deep agent planifie avec une liste de tâches, garde des notes dans des fichiers virtuels, confie des parties d'un travail à des sous-agents avec un contexte neuf et s'arrête avant les outils que vous nommez.
10. [Agents à long horizon](10-long-horizon.md): Un travail qui s'étend sur plusieurs sessions a besoin de compaction dans une session, de fichiers de passation et de mémoire entre les sessions, et d'une vérification de toute sa trajectoire.

https://agenticademy.org/fr/paths/advanced-agents
