# La boucle : observer, décider, agir

Les agents pour tous · Chapitre 2 sur 8 · 12 minutes

Chaque agent suit la même boucle : il observe ce qu'il sait, décide de l'étape suivante, agit, et observe à nouveau.

## Objectifs

- Prendre trois décisions comme si vous étiez le modèle.
- Amener l'agent à son objectif.
- Remettre dans l'ordre les étapes d'un tour.

## La lecture

### Une boucle, encore et encore

Au chapitre 1, vous avez vu un agent travailler par étapes. Chaque étape suit la même boucle en trois parties.

Observer : l'agent rassemble ce qu'il a jusque-là, c'est-à-dire votre demande et le résultat de sa dernière étape. Décider : le modèle lit tout cela et choisit l'étape suivante, ou décide que l'objectif est atteint. Agir : l'agent fait l'étape, en général en utilisant un outil, et un résultat revient. Puis il observe à nouveau.

Un passage par observer, décider et agir s'appelle un tour. Une petite tâche peut prendre trois ou quatre tours ; une grande peut en prendre des dizaines.

### Pourquoi il observe à nouveau

Ce qu'un outil renvoie est une information nouvelle. Si une recherche montre qu'un plombier est absent tout le mois, la décision suivante peut en tenir compte. C'est ainsi qu'un agent change de cap sans qu'on le lui dise : il lit le résultat avant de choisir à nouveau.

### Quand il s'arrête

Un agent s'arrête pour l'une de trois raisons. L'objectif est atteint. Il a besoin de vous : il pose une question ou demande votre approbation, et attend. Ou il atteint une limite, comme un nombre maximal de tours.

### Comment une boucle déraille

Une mauvaise décision coûte un tour. Réserver avant d'avoir trouvé quelqu'un, par exemple, ne mène à rien. Pire encore, la boucle qui s'emballe : l'agent répète la même étape, obtient le même résultat, et la répète encore. Sans limite, cela pourrait durer indéfiniment, et chaque tour utilise le modèle, ce qui prend du temps et coûte souvent de l'argent. C'est pourquoi les vrais agents comptent leurs tours et s'arrêtent à une limite.

### À vous

Ci-dessous, vous jouez le modèle. L'agent observe, puis vous décidez, puis l'agent agit et vous montre ce qui est revenu. Essayez un ou deux mauvais choix, et voyez ce qu'ils coûtent.

## Essayez : Vous êtes le modèle

Vous décidez de chaque étape pour l'agent. Regardez ce que chaque action renvoie, et voyez comment un mauvais choix coûte un tour de plus.

https://agenticademy.org/fr/paths/agents-for-everyone/02-the-agent-loop

## Vérification

Remettez dans l'ordre les parties d'un tour de la boucle.

1. Lire la demande et le résultat de la dernière étape.
2. Décider de l'étape suivante, ou décider que l'objectif est atteint.
3. Utiliser un outil pour faire cette étape.
4. Lire ce que l'outil a renvoyé.

**Pourquoi:** Observer, décider, agir, puis observer à nouveau : ce que l'outil renvoie fait partie de ce que l'agent observe au tour suivant.

## Ce que vous savez faire maintenant

- Décrire la boucle que suit tout agent : observer, décider, agir.
- Nommer trois raisons pour lesquelles un agent s'arrête : objectif atteint, il a besoin de vous, ou une limite.
- Expliquer pourquoi les agents ont besoin d'une limite sur le nombre d'étapes.

---

[Retour à Les agents pour tous](README.md) · [Suivant : Les outils : comment un agent atteint le monde](03-tools.md)
