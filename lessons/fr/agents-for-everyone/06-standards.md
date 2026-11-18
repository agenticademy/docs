# Les standards, en mots simples

Les agents pour tous · Chapitre 6 sur 8 · 14 minutes

Les standards sont des formats de messages convenus qui permettent à des agents, des outils, des applications et des boutiques créés par des personnes différentes de fonctionner ensemble.

## Objectifs

- Brancher le bon standard sur chaque connexion d'un parcours.
- Ouvrir un vrai message et lire ce que signifient ses champs.
- Réussir la vérification.

## La lecture

### Pourquoi des standards

Un standard est une façon convenue d'écrire des messages, un peu comme la forme d'une prise. Si tout le monde suit la même forme, n'importe quelle fiche entre dans n'importe quelle prise. Sans standards, chaque agent aurait besoin de son propre câblage sur mesure pour chaque application, outil et autre agent avec lequel il travaille. Avec eux, une entreprise construit son côté une fois et peut se connecter à beaucoup d'autres.

Un message, ici, est un petit texte structuré qu'un programme envoie à un autre : à qui il est destiné, ce qui est demandé, et les détails. Vous en verrez de vrais dans la simulation.

### Quatre types de connexion

Entre l'application d'une personne et un agent : AG-UI (Agent–User Interaction Protocol) transporte la conversation, y compris la réponse au fur et à mesure qu'elle s'écrit. A2UI (Agent-to-User Interface) permet à un agent de décrire un écran, comme un formulaire, que l'application dessine.

Entre agents : A2A (Agent2Agent Protocol) permet à un agent de confier une tâche à un autre, même à un agent géré par une autre entreprise, et de la suivre jusqu'à ce qu'elle soit terminée. ANP (Agent Network Protocol) sert aux agents qui se rencontrent sur le web ouvert.

Entre un agent et ses outils : MCP (Model Context Protocol) permet à un agent de trouver et d'utiliser des outils que quelqu'un d'autre fait tourner, comme la recherche de commandes d'un service d'assistance.

Entre un agent et des boutiques ou des paiements : UCP (Universal Commerce Protocol) donne aux agents une façon commune de passer commande dans une boutique, et AP2 (Agent Payments Protocol) permet à un agent de payer seulement avec la preuve que vous avez donné votre accord.

### Ce qu'est un standard, et ce qu'il n'est pas

Un standard n'est ni un produit, ni une application, ni une entreprise. C'est une description écrite de messages, et tout le monde peut s'en servir pour construire. Ces standards sont jeunes : le premier d'entre eux, MCP, est apparu fin 2024, et la plupart sont plus récents. Ils changent encore, alors quand vous lisez quelque chose sur l'un d'eux, vérifiez la date et la version.

## Essayez : Branchez les standards

Connectez le parcours lien par lien, chacun avec le standard conçu pour lui. Puis lancez le parcours, et ouvrez un vrai message.

https://agenticademy.org/fr/paths/agents-for-everyone/06-standards

## Vérification

### Question 1 sur 3

Quel standard relie un agent à ses outils ?

- A2A
- **✓** MCP
- AG-UI

**Pourquoi:** MCP, le Model Context Protocol, est la façon dont un agent trouve et appelle des outils que quelqu'un d'autre fait tourner. A2A relie des agents entre eux, et AG-UI relie une appli à un agent.

### Question 2 sur 3

À quoi sert A2A ?

- **✓** À un agent qui demande à un autre agent, géré par quelqu'un d'autre, de faire une tâche.
- À dessiner des boutons dans une appli.
- À payer dans une boutique.

**Pourquoi:** A2A (Agent2Agent) permet à un agent de confier une tâche à un autre et de la suivre jusqu'à ce qu'elle soit terminée. Dessiner des écrans est le rôle d'A2UI, et payer celui d'AP2.

### Question 3 sur 3

Qu'est-ce qu'un standard, dans ce sens ?

- Un produit que l'on achète.
- **✓** Un format convenu pour les messages, que tout le monde peut suivre.
- Une loi sur l'IA.

**Pourquoi:** Comme une forme de prise, un standard dit à quoi ressemblent les messages, pour que des pièces faites par des personnes différentes s'emboîtent. Ce n'est pas un produit, et ce n'est pas une loi.

## Ce que vous savez faire maintenant

- Expliquer ce qu'est un standard et pourquoi les agents en ont besoin.
- Nommer le standard de chaque type de connexion : applis, agents, outils, paiements.
- Lire, avec de l'aide, les principaux champs d'un vrai message d'agent.

---

[Retour à Les agents pour tous](README.md) · [Suivant : Risques, sécurité et savoir dire oui](07-risks-and-approval.md)
