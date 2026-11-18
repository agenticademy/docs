# Agents et interfaces : AG-UI et A2UI

Systèmes d'agents et standards · Chapitre 3 sur 8 · 25 minutes

Diffuser le travail d'un agent vers une app avec AG-UI, et laisser un agent décrire des composants d'interface sûrs avec A2UI.

## Objectifs

- Regardez le flux d'événements AG-UI d'une exécution jusqu'au bout.
- Confirmez le remboursement par un outil front-end.
- Injectez un composant hors du catalogue A2UI et voyez-le refusé.
- Réussissez la vérification.

## La lecture

### AG-UI : le travail de l'agent en flux

MCP relie un agent à des outils et A2A à d'autres agents. AG-UI (Agent–User Interaction protocol) le relie à l'app qu'utilise une personne. L'app envoie un RunAgentInput (id de thread, id d'exécution, messages, état et outils que l'app propose) et lit en retour un flux d'événements, ici sous forme de server-sent events.

Une exécution commence par RUN\_STARTED et se termine par RUN\_FINISHED ou RUN\_ERROR. Le texte arrive en TEXT\_MESSAGE\_START, puis en fragments TEXT\_MESSAGE\_CONTENT, puis en TEXT\_MESSAGE\_END. Les appels d'outils arrivent de la même façon : TOOL\_CALL\_START, TOOL\_CALL\_ARGS, TOOL\_CALL\_END, et TOOL\_CALL\_RESULT pour les outils que l'agent a exécutés. L'état partagé circule en STATE\_SNAPSHOT, l'état complet, ou en STATE\_DELTA, une liste d'opérations JSON Patch. Le client officiel, @ag-ui/client 1.0.2, vérifie le schéma et l'ordre de chaque événement avant de l'appliquer.

### Les outils front-end

Certains outils appartiennent à l'app : un bouton de confirmation, un sélecteur de date. L'app les liste dans RunAgentInput.tools. Quand l'agent en appelle un, l'exécution se termine avec cet appel en attente ; l'app exécute l'outil, ici en interrogeant le client, et lance l'exécution suivante avec le résultat comme message d'outil. La réponse de la personne entre dans la conversation, et l'agent décide quoi en faire.

### A2UI : des interfaces sous forme de données

A2UI (protocole v0.9) permet à un agent de décrire un morceau d'interface sans envoyer de code. createSurface ouvre une surface et nomme le catalogue qu'elle utilise ; updateComponents envoie une liste plate de composants qui se désignent les uns les autres par id ; updateDataModel envoie les valeurs auxquelles ils sont liés. L'app n'affiche que des composants de son propre catalogue, ici sept types : Card, Column, Row, List, Text, Button et Divider. Avec la validation stricte, @a2ui/web\_core 0.12.0 refuse un type inconnu et la surface reste telle quelle, si bien qu'un agent, ou un prompt qui l'a trompé, ne peut pas faire exécuter de nouveau code à l'app.

### Ce que vous faites ici

Un client interroge le chat de support sur le double prélèvement. Par AG-UI, regardez le flux d'événements, puis confirmez le remboursement sur la carte que dessine l'app. Passez ensuite le lab à A2UI, où l'agent construit une carte de remboursement à partir de messages. Activez Intercepter, retenez le message updateComponents et ajoutez un composant Script : web\_core le refuse.

## Essayez : Un agent construit un écran

Quatre étapes, une action chacune, puis deux facultatives. Le lab sous les étapes dessine chaque message comme une ligne, de haut en bas. Tant qu'un lab A2UI est ouvert, un arbre sous le lab montre les composants que l'agent a envoyés, reliés par id.

https://agenticademy.org/fr/paths/agent-systems/03-agents-and-uis

## Vérification

### Question 1 sur 4

Qu'est-ce qu'un outil front-end dans AG-UI ?

- **✓** Un outil que l'app exécute, comme une boîte de confirmation ; son résultat revient à l'agent à l'exécution suivante.
- Un outil qui dessine des graphiques sur le serveur.
- Une extension de navigateur que l'agent installe.

**Pourquoi:** L'agent le demande comme n'importe quel autre outil, mais c'est l'app qui le déclare et l'exécute, si bien qu'une personne peut prendre part à l'étape. L'exécution se termine avec l'appel en attente ; le résultat lance l'exécution suivante.

### Question 2 sur 4

Pourquoi A2UI affiche-t-il à partir d'un catalogue au lieu d'exécuter le balisage de l'agent ?

- C'est plus rapide.
- **✓** C'est l'app qui décide quels composants existent, donc un agent, ou un texte qu'on lui a fourni, ne peut pas faire exécuter de nouveau code à l'app.
- Le HTML exige des catalogues.

**Pourquoi:** Décrire l'UI sous forme de données laisse l'app aux commandes. Avec la validation stricte, un type de composant hors du catalogue est refusé, quoi que l'agent envoie.

### Question 3 sur 4

Que transporte un événement STATE\_DELTA d'AG-UI ?

- Une copie complète de l'état.
- **✓** Des opérations JSON Patch à appliquer à l'état partagé.
- L'utilisation de tokens du modèle.

**Pourquoi:** Les deltas gardent les mises à jour petites. Si un patch vise un chemin qui n'existe pas, le client ne peut pas l'appliquer et garde son état précédent ; un STATE\_SNAPSHOT le remet en phase.

### Question 4 sur 4

Le client reçoit TEXT\_MESSAGE\_CONTENT pour un messageId qui n'a jamais eu de TEXT\_MESSAGE\_START. Que se passe-t-il ?

- Le texte s'affiche quand même.
- **✓** @ag-ui/client rejette l'événement comme hors d'ordre et arrête l'exécution.
- L'agent le renvoie.

**Pourquoi:** Le client vérifie l'ordre des événements autant que leur schéma. Du contenu pour un message qui n'a jamais commencé est une erreur de protocole, donc l'exécution s'arrête.

## Ce que vous savez faire maintenant

- Lire un flux d'événements AG-UI et dire ce que fait chaque événement.
- Utiliser un outil front-end pour faire intervenir une personne dans l'exécution d'un agent.
- Décrire une surface A2UI : createSurface, updateComponents et updateDataModel.
- Expliquer comment le catalogue d'A2UI protège une app d'une UI fabriquée par un agent.

---

[Retour à Systèmes d'agents et standards](README.md) · [Suivant : Des agents qui paient : UCP et AP2](04-agent-commerce.md)
