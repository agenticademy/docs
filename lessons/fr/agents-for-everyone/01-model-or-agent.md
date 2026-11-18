# Un modèle répond. Un agent agit.

Les agents pour tous · Chapitre 1 sur 8 · 10 minutes

La différence entre un modèle d'IA, qui vous donne du texte, et un agent, qui fait des étapes pour atteindre un objectif.

## Objectifs

- Envoyer la même demande à un modèle et à un agent, et les laisser finir tous les deux.
- Trouver l'étape où l'agent a changé quelque chose hors de la conversation.
- Réussir la vérification.

## La lecture

### Un modèle écrit

Un modèle d'IA, comme celui qui fait tourner un chatbot, est un programme entraîné sur une très grande quantité de texte. À partir de quelques mots, il prédit les mots qui devraient suivre. Cela suffit pour répondre à une question, résumer une lettre ou rédiger un e-mail.

Les modèles sont créés par des entreprises et des groupes de recherche. Certains modèles sont ouverts : tout le monde peut les télécharger et les faire tourner. Beaucoup sont partagés sur Hugging Face, un site de partage de modèles, et certains, comme la famille Nemotron de NVIDIA, peuvent être essayés gratuitement, avec des limites.

Seul, un modèle ne peut que vous rendre du texte. Il ne peut pas consulter votre agenda ni réserver quoi que ce soit. Quoi qu'il propose, c'est encore à vous de le faire.

### Un agent fait

Un agent est un programme construit autour d'un modèle. Vous lui donnez un objectif, et il peut utiliser des outils : d'autres programmes qui cherchent des informations ou changent des choses, comme un agenda, une carte ou un site de réservation.

L'agent travaille par étapes. Le modèle décide quoi faire ensuite, l'agent utilise l'outil, et le modèle lit ce qui est revenu. Cela se répète jusqu'à ce que l'objectif soit atteint, ou que l'agent ait besoin de vous. « IA agentique » est le nom général des systèmes qui fonctionnent ainsi.

### Même demande, deux résultats

Demandez à un modèle de réserver une table pour six samedi, et il vous explique comment en réserver une. Demandez à un agent, et il peut vérifier votre agenda, trouver un restaurant, vous demander de confirmer et faire la réservation. La simulation ci-dessous joue les deux, pour que vous puissiez les comparer.

### Pourquoi la différence compte

Un texte, on peut l'ignorer facilement. Une action, non : une réservation, un paiement ou un e-mail envoyé a des effets dans le monde. C'est pourquoi un bon agent demande avant de faire quelque chose d'important, et pourquoi ce parcours consacre un chapitre entier à la sécurité.

### Quatre mots pour ce parcours

Modèle : le programme qui lit et écrit du texte. Outil : ce qu'un agent peut utiliser pour chercher des informations ou pour agir. Objectif : ce que vous avez demandé à l'agent. Étape : une décision du modèle, et une action de l'agent.

## Essayez : Une demande, deux résultats

Faites la même demande à un modèle, puis à un agent. Répondez à l'agent quand il vous demande, puis trouvez l'étape qui a changé quelque chose dans le monde.

https://agenticademy.org/fr/paths/agents-for-everyone/01-model-or-agent

## Vérification

### Question 1 sur 4

Vous demandez une recette à un chatbot et il l'écrit. Est-ce un agent au travail ?

- Oui, parce qu'il a utilisé l'IA.
- **✓** Non. Il a rendu du texte, et rien n'a été fait pour vous.
- Oui, parce qu'il a compris ce que vous vouliez dire.

**Pourquoi:** Écrire une recette, c'est ce qu'un modèle fait seul. Un agent utiliserait des outils pour agir, par exemple vérifier ce qu'il y a dans votre commande de courses en ligne et ajouter les articles manquants.

### Question 2 sur 4

Qu'est-ce qu'un agent a, qu'un modèle seul n'a pas ?

- Une plus grande mémoire d'internet.
- **✓** Des outils qu'il peut utiliser, et une boucle d'étapes qui continue jusqu'à ce que l'objectif soit atteint.
- Une voix et un visage.

**Pourquoi:** Un agent, c'est un modèle plus des outils plus une boucle : décider, agir, lire le résultat, décider à nouveau. Le modèle à l'intérieur peut être exactement le même que celui d'un chatbot.

### Question 3 sur 4

Dans la simulation, pourquoi l'agent vous a-t-il demandé avant de réserver ?

- **✓** Une réservation a des effets réels, donc une personne doit d'abord la confirmer.
- Il ne savait pas comment réserver.
- Les agents demandent avant chaque étape.

**Pourquoi:** Chercher des informations est sans risque ; réserver ne l'est pas. Les bons agents demandent avant les actions difficiles à annuler ou qui vous engagent, et font le reste eux-mêmes.

### Question 4 sur 4

Lequel de ces éléments est un outil, au sens utilisé ici ?

- Le texte sur lequel le modèle a été entraîné.
- Votre demande.
- **✓** Un agenda que l'agent peut lire.

**Pourquoi:** Un outil est ce que l'agent peut utiliser pendant ses étapes pour chercher des informations ou pour agir. Votre demande est l'objectif, et le texte d'entraînement est ce dont le modèle a appris.

## Ce que vous savez faire maintenant

- Distinguer un modèle d'un agent avec une seule question : peut-il agir, ou seulement répondre ?
- Nommer les parties d'un agent : un modèle, des outils, un objectif et une boucle d'étapes.
- Expliquer pourquoi un agent doit vous demander avant une action importante.

---

[Retour à Les agents pour tous](README.md) · [Suivant : La boucle : observer, décider, agir](02-the-agent-loop.md)
