# MCP : des outils fournis par un serveur

Systèmes d'agents et standards · Chapitre 1 sur 8 · 25 minutes

Comment un agent trouve et appelle les outils d'un serveur MCP, et comment le protocole 2026-07-28 demande l'avis d'une personne avant un appel risqué.

## Objectifs

- Lancez le scénario jusqu'à ce que le remboursement soit effectué.
- Répondez vous-même à la confirmation du helpdesk.
- Altérez un message et voyez le vrai SDK le rejeter.
- Réussissez la vérification.

## La lecture

### Hôtes, clients et serveurs

Le Model Context Protocol (MCP) est un standard ouvert pour relier des applications d'IA à des outils et à des données. L'application, appelée l'hôte, fait tourner un client MCP par serveur qu'elle utilise. Un serveur propose des outils (des actions que le modèle peut appeler), des ressources (des données que l'application peut lire) et des prompts (des modèles de texte qu'une personne peut choisir). Les messages sont en JSON-RPC 2.0, sur HTTP ou sur l'entrée et la sortie standard d'un processus local (stdio).

Dans l'agent du parcours précédent, les outils étaient des fonctions de votre code. Avec MCP, ils vivent dans un serveur que quelqu'un d'autre peut écrire et faire tourner, et que n'importe quel hôte MCP peut utiliser.

### Ce que 2026-07-28 a changé

Ce chapitre utilise la version 2026-07-28 du protocole avec le SDK TypeScript officiel v2. Vous verrez trois changements sur le fil.

La découverte : le client appelle server/discover pour connaître les versions et les capacités du serveur. Les serveurs des versions 2025 répondent plutôt à initialize, et le SDK se rabat dessus.

Une version sur chaque requête : chaque requête porte sa version du protocole dans \_meta, si bien que le serveur ne garde aucune session et que n'importe laquelle de ses instances peut répondre à n'importe quelle requête.

Saisie requise : quand un outil a besoin de la réponse d'une personne, le serveur termine la requête avec input\_required, la question et un requestState signé. Le client pose la question à la personne, puis rappelle l'outil avec la réponse et le même requestState. Le serveur vérifie la signature, donc la nouvelle tentative doit correspondre à ce que la personne a vu.

### Les outils sont un contrat

Le modèle ne voit que le nom d'un outil, sa description et son JSON Schema. Les annotations comme readOnlyHint et destructiveHint sont des indications du serveur : un client ne s'y fie que dans la mesure où il fait confiance à ce serveur. Dans ce lab, le remboursement est sûr parce que le serveur lui-même demande une confirmation et la vérifie, pas parce que le modèle se comporte bien.

### Ce que vous faites ici

L'agent de support de Halden, un agent LangChain avec le client du SDK, et le helpdesk de Halden, le serveur du SDK, tournent tous les deux dans cette page. Lancez le scénario et répondez vous-même à la question sur le remboursement. Ensuite, activez Intercepter, retenez un message et altérez-le : le vrai code du SDK le rejette.

## Essayez : Le fil : un agent et un helpdesk MCP

Quatre étapes, une action chacune, et une cinquième facultative. Le diagramme sous les étapes a une colonne par participant et une ligne par message, dans l'ordre d'envoi, de haut en bas.

https://agenticademy.org/fr/paths/agent-systems/01-mcp

## Vérification

### Question 1 sur 4

Dans MCP, qui fait tourner le client ?

- Le serveur.
- **✓** L'application hôte, par exemple l'agent de support.
- Le fournisseur du modèle.

**Pourquoi:** L'hôte, par exemple un agent ou un IDE, fait tourner un client par serveur auquel il se connecte. Le modèle ne parle jamais directement au serveur ; c'est l'hôte qui le fait, quand le modèle demande un outil.

### Question 2 sur 4

Le helpdesk a besoin de l'accord du responsable du support avant un remboursement. Qu'envoie un serveur 2026-07-28 ?

- Une nouvelle requête du serveur vers le client, qui demande une saisie.
- **✓** Un résultat avec input\_required, la question et un requestState signé, qui termine la requête.
- L'erreur -32602.

**Pourquoi:** Le serveur termine la requête au lieu d'attendre sur une connexion ouverte. Le client interroge la personne et refait l'appel avec la réponse et le même requestState, si bien que n'importe quelle instance du serveur peut finir le travail.

### Question 3 sur 4

Lors de la nouvelle tentative, vous augmentez le montant du remboursement. Pourquoi le serveur la rejette-t-il ?

- Le modèle remarque le changement.
- **✓** Les arguments ne correspondent plus au requestState signé, qui a été émis pour ce que la personne a approuvé.
- MCP n'autorise pas les montants dans les arguments.

**Pourquoi:** requestState est signé par le serveur (ici avec HMAC). Le serveur relit la commande et le montant approuvés et refuse tout appel qui en diffère.

### Question 4 sur 4

Un serveur marque un outil avec readOnlyHint: true. Que doit faire le client ?

- L'exécuter sans jamais rien demander à personne, puisqu'il ne peut rien modifier.
- **✓** Le traiter comme une indication, à laquelle il ne se fie que dans la mesure où il fait confiance à ce serveur.
- Refuser de l'exécuter.

**Pourquoi:** Les annotations décrivent ce que le serveur affirme. Un serveur malveillant ou bogué peut mal étiqueter un outil, donc c'est la politique propre au client qui décide de ce qui demande une approbation.

## Ce que vous savez faire maintenant

- Décrire un hôte, un client et un serveur MCP, et les trois choses qu'un serveur propose.
- Suivre le flux 2026-07-28 : server/discover, tools/list, tools/call, input\_required et la nouvelle tentative.
- Expliquer pourquoi un requestState signé garde une confirmation liée à ce que la personne a vu.
- Traiter les annotations des outils comme des indications, pas comme des garanties.

---

[Retour à Systèmes d'agents et standards](README.md) · [Suivant : A2A : des agents qui délèguent à des agents](02-a2a.md)
