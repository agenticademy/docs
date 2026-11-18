# Systèmes d'agents et standards

Intermédiaire · Pour les développeurs · 8 chapitres · 195 min

Pour les développeurs qui savent déjà construire un agent seul. Vous reliez des agents à des outils, à d'autres agents, à des interfaces et à des paiements avec les standards ouverts, vous orchestrez plusieurs agents dans un même graphe, vous décidez de ce qui entre dans chaque contexte, vous tracez les exécutions et vous les testez contre des attaques. Les bibliothèques des protocoles tournent pour de vrai dans votre navigateur ; vous pouvez retenir, supprimer et altérer chaque message.

## Avant de commencer

Le parcours Construire vos premiers agents, ou les mêmes connaissances : les messages et outils de LangChain.js, les graphes LangGraph et la boucle de l'agent.

## À la fin, vous saurez

- Relier un agent à un serveur MCP avec le protocole 2026-07-28, y compris les demandes de saisie à une personne.
- Déléguer du travail à un autre agent avec les tâches A2A et les Agent Cards.
- Diffuser le travail d'un agent vers une interface avec AG-UI, et décrire une UI sûre avec A2UI.
- Expliquer comment UCP et AP2 lient l'achat d'un agent au consentement signé d'une personne.
- Orchestrer des agents avec des superviseurs, le fan-out avec Send et des sous-graphes.
- Concevoir le contexte avec la recherche, l'éviction et des sous-agents, et tracer des exécutions au format de LangSmith.
- Tester un système d'agents contre l'injection de prompt et l'altération, avec les risques agentiques de l'OWASP comme carte.
- Lire une chronologie de supersteps et un arbre d'exécutions.
- Ajouter à un agent une limite d'appels, une limite d'outils ou une protection sous forme de middleware.

## Chapitres

1. [MCP : des outils fournis par un serveur](01-mcp.md): Comment un agent trouve et appelle les outils d'un serveur MCP, et comment le protocole 2026-07-28 demande l'avis d'une personne avant un appel risqué.
2. [A2A : des agents qui délèguent à des agents](02-a2a.md): Comment un agent en trouve un autre grâce à son Agent Card et lui confie une tâche qu'il peut suivre jusqu'au bout.
3. [Agents et interfaces : AG-UI et A2UI](03-agents-and-uis.md): Diffuser le travail d'un agent vers une app avec AG-UI, et laisser un agent décrire des composants d'interface sûrs avec A2UI.
4. [Des agents qui paient : UCP et AP2](04-agent-commerce.md): Comment un agent passe commande chez un marchand avec UCP, et comment les mandats AP2 lient le paiement à ce que la personne a signé.
5. [Orchestration : superviseurs, fan-out, sous-graphes](05-orchestration.md): Des patterns pour plusieurs agents dans un même graphe LangGraph : un superviseur qui oriente, Send pour le travail en parallèle, les sous-graphes, et le checkpoint que chaque superstep enregistre.
6. [Ingénierie du contexte et récupération](06-context-engineering.md): Choisir ce que lit chaque appel au modèle : grep ou recherche vectorielle, déchargement des gros résultats, un premier aperçu des sous-agents, et une mémoire qui dure d'un thread à l'autre.
7. [Observabilité : traces et exécutions](07-observability.md): Enregistrer chaque appel au modèle, chaque appel d'outil et chaque étape sous forme d'arbre d'exécutions, et le lire pour trouver ce qui est lent ou cassé.
8. [Évaluation et sécurité](08-evaluation-and-safety.md): Tester un système d'agents avec des cas et des contrôles, l'attaquer par injection et altération, et rapporter les échecs aux risques agentiques de l'OWASP.

https://agenticademy.org/fr/paths/agent-systems
