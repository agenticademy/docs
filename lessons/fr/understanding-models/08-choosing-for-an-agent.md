# Choisir un modèle pour un agent

Comprendre les modèles · Chapitre 8 sur 8 · 18 minutes

Choisir un modèle, c'est écarter ceux qui échouent à une limite stricte, puis peser la capacité, la vitesse, le prix par tâche, le contexte et la fiabilité avec les outils pour le travail d'un agent.

## Objectifs

- Choisir un agent pour lequel choisir un modèle.
- Appliquer ses limites strictes et voir quels modèles sont exclus.
- Peser ce qui reste, le classer et chiffrer un mois de travail.
- Réussir la vérification.

## La lecture

### Le bon choix, pas le meilleur

Il n'y a pas de meilleur modèle, seulement celui qui convient le mieux au travail d'un agent. Le choix se fait en deux passes. D'abord les limites strictes : des besoins dont l'agent ne peut pas se passer. Un modèle qui échoue à l'une d'elles est exclu, aussi fort ou bon marché soit-il. Ensuite, vous pesez ce qui reste : vous décidez combien chaque qualité compte pour ce travail, et vous classez les modèles en fonction.

### Limites strictes

Modalité : les types d'entrée et de sortie qu'un modèle gère, comme le texte, les images ou le son. Fenêtre de contexte : combien de tokens il lit à la fois, comme au chapitre 2. Politique de données : si le fournisseur peut entraîner ses modèles sur ce que vous envoyez, et la conservation, combien de temps il le garde. Région : où vos données sont traitées ; certaines règles, comme celles de l'UE, s'en soucient. Licence et accès : ce que vous pouvez faire avec un modèle, et qui peut l'utiliser tout court. Les poids ouverts peuvent aussi tourner sur votre propre machine, même sans internet.

### Ce que vous pesez

Capacité : à quel point il fait bien le travail. Les benchmarks, comme au chapitre 7, aident quand leurs tâches sont proches des vôtres, mais votre propre évaluation en dit plus : quelques dizaines de tâches réelles du travail, notées par vous. Latence : combien de temps vous attendez une réponse ; plus de réflexion donne en général de meilleures réponses et des attentes plus longues.

Prix par tâche : ce que coûte une tâche terminée. Un agent appelle son modèle plusieurs fois par tâche, et chaque appel est un tour, facturé pour ses tokens en entrée et en sortie. Un prix par token bas peut quand même donner une tâche chère. Fiabilité des appels d'outils : à quelle fréquence le modèle appelle correctement les outils, avec les bonnes entrées. Limites de débit : combien de requêtes et de tokens un fournisseur accepte par minute.

### Dans le visuel

Vous choisissez l'un de quatre agents. Ses limites strictes écartent des modèles, vos priorités classent le reste, et un calculateur transforme de vrais prix catalogue en facture mensuelle. Une réponse commentée montre ensuite ce à quoi son choix renonce. Les agents et la taille de leurs tâches sont inventés. Les modèles, prix, fenêtres de contexte, entrées, licences et scores sont réels, tirés d'un instantané daté du 2026-11-03. L'instantané ne contient pas de régions et seulement certaines conditions sur les données, donc certaines limites indiquent « À vérifier » ; il ne contient ni vitesses ni limites de débit, donc vous les testez vous-même.

## Essayez : Choisissez un modèle pour cet agent

Choisissez un agent, appliquez ses limites strictes une à une, pesez ce qui reste et chiffrez un mois de travail, puis comparez avec une réponse commentée.

https://agenticademy.org/fr/paths/understanding-models/08-choosing-for-an-agent

## Vérification

### Question 1 sur 3

Un bot de support pour une banque européenne doit garder les données clients dans l'UE. Deux modèles peuvent faire le travail ; le moins cher ne tourne que sur des serveurs hors de l'UE. Qu'est-ce qui vient en premier ?

- Prendre le moins cher : le prix compte le plus.
- **✓** Écarter celui qui ne peut pas garder les données dans l'UE, puis comparer le reste.
- Prendre celui qui a le meilleur score au classement.
- Utiliser les deux et les laisser voter.

**Pourquoi:** Garder les données dans l'UE est une limite stricte : un modèle qui y échoue est exclu, aussi bon marché ou fort soit-il. Le prix et les scores ne font que classer les modèles autorisés. Laisser les deux voter enverrait quand même les données hors de l'UE.

### Question 2 sur 3

Le modèle B coûte deux fois plus par token que le modèle A. Dans votre évaluation, B termine une tâche en 2 tours, et A a besoin de 5 tours de même taille. Lequel coûte le moins par tâche ?

- A, parce que son prix par token est la moitié de celui de B.
- **✓** B, parce que 2 tours à prix double coûtent moins que 5 tours.
- Aucun : avec les mêmes tokens par tour, c'est le prix par token qui décide.
- Impossible à dire sans score de classement.

**Pourquoi:** Le prix par tâche est le prix par token multiplié par les tokens qu'une tâche utilise. Si un tour coûte 1 sur A, il coûte 2 sur B, donc la tâche de A coûte 5 et celle de B coûte 4. Le prix par token seul ne compte pas les tours, et un score de classement ne compte pas du tout vos tours ; votre propre évaluation, si.

### Question 3 sur 3

Le premier modèle de votre présélection a le meilleur score indépendant, mais l'instantané ne dit rien de sa vitesse ni de la fiabilité de ses appels d'outils. Que faites-vous avant de vous engager ?

- Rien : le meilleur score veut dire qu'il est aussi le plus rapide et le plus fiable.
- **✓** Lancer votre propre évaluation : de vraies tâches du travail, en chronométrant les réponses et en vérifiant chaque appel d'outil.
- Prendre plutôt le modèle le moins cher.
- Attendre le classement du mois prochain.

**Pourquoi:** Un score mesure les tâches de son benchmark à un seul réglage ; il ne chronomètre pas les réponses sur votre travail et ne vérifie pas vos outils. Votre propre évaluation, quelques dizaines de tâches réelles que vous notez vous-même, mesure exactement cela. Choisir sur le seul prix, ou attendre un classement plus récent, laisse le même manque.

## Ce que vous savez faire maintenant

- Séparer les limites strictes qui écartent un modèle des qualités que l'on pèse.
- Calculer un prix par tâche et par mois à partir des tokens, des tours et des prix catalogue.
- Classer une présélection selon ce qui compte pour un agent, et dire ce à quoi le choix renonce.
- Nommer ce que la page d'un modèle ne peut pas vous dire, et le tester dans votre propre évaluation.

---

[Retour à Comprendre les modèles](README.md)
