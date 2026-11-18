# Construisez vos premiers agents

Débutant · Pour les développeurs · 8 chapitres · 152 min

Pour les développeurs qui débutent avec les agents. Vous construisez pas à pas un petit agent de support : un appel au modèle, les messages et l'état, un graphe, des outils, la boucle de l'agent, des callbacks avec approbation humaine, et des garde-fous. Chaque chapitre exécute les vraies bibliothèques dans votre navigateur avec un modèle scripté, vous n'avez donc besoin d'aucune clé ; vous pouvez connecter votre propre modèle quand vous le souhaitez.

## Avant de commencer

Un peu de JavaScript : fonctions, objets, async/await et installation de paquets avec npm.

## À la fin, vous saurez

- Choisir les paquets LangChain.js et LangGraph.js d'un agent, et savoir ce que fait chacun.
- Appeler un modèle de chat et manipuler les messages et l'état du graphe.
- Construire un StateGraph avec des nœuds, des arêtes et un routage conditionnel.
- Définir des outils avec des schémas zod et faire tourner un agent ReAct minimal.
- Mettre une exécution en pause pour une approbation humaine avec des checkpoints et interruptBefore, et la journaliser avec des callbacks.
- Ajouter des garde-fous : contrôles d'entrée et de sortie, listes d'autorisation d'outils et limites d'étapes.

## Chapitres

1. [Les bibliothèques, et ce qui tourne ici](01-libraries-and-setup.md): Les paquets LangChain.js et LangGraph.js dont un agent a besoin, le rôle de chacun, et leur fonctionnement dans un navigateur.
2. [Un appel au modèle](02-one-model-call.md): Appeler un modèle de chat avec LangChain.js, lire l'AIMessage qu'il renvoie et utiliser votre propre clé en sécurité.
3. [Messages et état](03-messages-and-state.md): Les quatre types de messages, pourquoi toute la conversation est envoyée à chaque fois, et comment LangGraph fusionne l'état avec des reducers.
4. [Votre premier graphe](04-your-first-graph.md): Construire un StateGraph avec des nœuds et des arêtes, router avec une arête conditionnelle et l'exécuter pas à pas.
5. [Outils et appels d'outils](05-tools-and-tool-calls.md): Définir des outils avec zod, les lier à un modèle et traiter les appels d'outils que fait le modèle.
6. [La boucle de l'agent : un agent ReAct minimal](06-the-agent-loop.md): Un nœud modèle, un nœud outils et une arête conditionnelle : toute la boucle de l'agent sous forme de graphe LangGraph.
7. [Callbacks et approbation humaine](07-callbacks-and-approval.md): Observer une exécution avec des callbacks, et la mettre en pause avant un outil risqué pour qu'une personne l'approuve ou la refuse.
8. [Garde-fous](08-guardrails.md): Des contrôles dans le code autour du modèle : contrôles d'entrée et de sortie, listes d'autorisation d'outils et limites d'étapes, en couches.

https://agenticademy.org/fr/paths/first-agents
