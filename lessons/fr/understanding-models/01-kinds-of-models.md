# Les types de modèles

Comprendre les modèles · Chapitre 1 sur 8 · 14 minutes

Les modèles de chat, de raisonnement, multimodaux et d'embedding, les rerankers, les modèles vocaux, d'image et de garde, et les petits modèles font des tâches différentes, et un agent en utilise souvent plusieurs.

## Objectifs

- Remplir le premier emplacement de l'agent vocal : transformer la voix de l'appelant en texte.
- Remplir les deux emplacements qui trouvent les fiches répondant à la question.
- Remplir les trois derniers emplacements : écrire la réponse, la vérifier et la dire.
- Réussir la vérification.

## La lecture

### Des modèles différents pour des tâches différentes

Un modèle est un programme qui a appris des motifs à partir d'exemples. Les modèles apprennent des tâches différentes. Chacun reçoit un certain type d'entrée, comme du texte, du son ou des images, et produit un certain type de sortie. Un agent enchaîne souvent plusieurs modèles, chacun faisant la tâche qu'il fait le mieux.

### Les modèles qui écrivent

Un modèle de chat, aussi appelé modèle instruct, suit des instructions et écrit une réponse. Un modèle de raisonnement travaille un problème étape par étape avant de répondre : il est meilleur sur les problèmes difficiles, mais plus lent. Beaucoup de nouveaux modèles font les deux, avec un réglage du temps qu'ils passent à réfléchir. Un modèle multimodal lit aussi des images, du son ou de la vidéo.

### Les modèles qui trouvent et trient

Un modèle d'embedding transforme un texte en une liste de nombres, de sorte que des textes de sens proche reçoivent des nombres proches. Une recherche peut alors comparer les nombres et trouver des fiches de sens proche d'une question, rapidement, même parmi des milliers. Un reranker lit la question avec chaque fiche qu'on lui donne et met la meilleure en tête. C'est plus soigneux mais plus lent, donc il ne trie que les quelques fiches que la recherche a trouvées.

### Les modèles qui écoutent, parlent, dessinent et vérifient

La reconnaissance vocale écrit ce que quelqu'un dit. La synthèse vocale lit un texte à voix haute. Un modèle d'image crée une image à partir d'une description. Un modèle de garde, aussi appelé classifieur de sécurité, étiquette des textes ou des images comme sûrs ou dangereux, pour qu'un agent puisse arrêter une mauvaise réponse avant que quelqu'un la voie ou l'entende.

### Petits, fermés et ouverts

La taille d'un modèle se compte en paramètres : les valeurs internes qu'il a ajustées pendant l'entraînement, aussi appelées ses poids. Les petits modèles ont quelques milliards de paramètres ou moins, donc ils peuvent tourner sur un téléphone ou un ordinateur portable. Les poids d'un modèle fermé ne sont pas publiés, et la plupart des modèles fermés s'utilisent via un service en ligne. Un modèle à poids ouverts peut être téléchargé et exécuté sur votre propre ordinateur.

Dans le visuel, vous équipez un agent vocal pour la ligne téléphonique d'une bibliothèque, un emplacement à la fois. La bibliothèque est inventée ; chaque modèle d'exemple que vous rencontrez est réel, au 2026-11-03.

## Essayez : Équiper un agent vocal

Remplissez un emplacement à la fois, en partant du haut : lisez ce que l'emplacement reçoit et ce qu'il doit produire, puis choisissez la carte de modèle qui correspond.

https://agenticademy.org/fr/paths/understanding-models/01-kinds-of-models

## Vérification

### Question 1 sur 3

Un agent d'assistance doit trouver, parmi 5 000, les trois articles d'aide qui correspondent le mieux à la question d'un client. Quelle paire de modèles est faite pour cette tâche ?

- Un modèle d'image et un modèle de reconnaissance vocale.
- **✓** Un modèle d'embedding pour trouver les articles de sens proche, puis un reranker pour mettre le meilleur en tête.
- Un modèle de garde et un modèle de synthèse vocale.
- Un reranker seul, qui lit les 5 000 articles avec la question.

**Pourquoi:** Les embeddings permettent à une recherche de comparer des nombres, donc elle trouve vite des articles similaires, même parmi des milliers. Un reranker lit ensuite la question avec chacun des quelques articles trouvés et met le meilleur en tête. Un reranker seul lirait les 5 000 paires pour chaque question, ce qui est lent. Les modèles d'image, de reconnaissance vocale, de garde et de synthèse vocale font d'autres tâches : images, son et étiquettes de sécurité.

### Question 2 sur 3

Une application de terrain doit répondre à des questions sur un téléphone sans connexion internet. Quel type de modèle peut le faire ?

- Un modèle de chat fermé, utilisé via le service de son créateur.
- **✓** Un petit modèle à poids ouverts, téléchargé sur le téléphone.
- Un modèle de raisonnement sur un serveur.
- Un modèle d'embedding seul.

**Pourquoi:** Sans connexion, le modèle doit tourner sur le téléphone lui-même. Un petit modèle à poids ouverts le peut : vous le téléchargez une fois, et il tourne sans internet. Un modèle de chat fermé utilisé via le service de son créateur et un modèle de raisonnement sur un serveur ont tous deux besoin d'une connexion pour être joints. Un modèle d'embedding produit des nombres, pas des réponses.

### Question 3 sur 3

Un appelant pose une question facile à un agent vocal et attend au bout du fil. Pourquoi un modèle de chat convient-il mieux qu'un modèle de raisonnement à l'emplacement de la réponse ?

- Un modèle de raisonnement ne sait pas lire du texte.
- **✓** Un modèle de raisonnement passe par des étapes avant de répondre, donc l'appelant attend plus longtemps une réponse facile.
- Un modèle de chat a toujours raison.
- Un modèle de raisonnement produit du son, pas du texte.

**Pourquoi:** Les deux reçoivent du texte et produisent du texte. Un modèle de raisonnement passe du temps à réfléchir avant de répondre, ce qui aide sur les problèmes difficiles mais laisse l'appelant dans le silence sur une question facile. Aucun modèle n'a toujours raison, et transformer du texte en son est la tâche de la synthèse vocale.

## Ce que vous savez faire maintenant

- Nommer dix types de modèles et dire ce que chacun reçoit et produit.
- Choisir un type de modèle pour chaque étape d'un agent simple, et dire pourquoi un autre type ne conviendrait pas.
- Donner un exemple fermé et un exemple ouvert de chaque type, à partir d'un instantané daté.

---

[Retour à Comprendre les modèles](README.md) · [Suivant : Les tokens](02-tokens.md)
