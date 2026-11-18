# Les outils : comment un agent atteint le monde

Les agents pour tous · Chapitre 3 sur 8 · 12 minutes

Les outils permettent à un agent de chercher des informations et d'accomplir des choses, et le modèle ne connaît chaque outil que par son étiquette.

## Objectifs

- Donner à l'agent le bon outil pour trois demandes.
- Voir une description d'outil floue conduire l'agent au mauvais outil.
- Réussir la vérification.

## La lecture

### Ce qu'est un outil

Un outil est un programme qu'un agent peut utiliser pour faire ce qu'il ne peut pas faire avec du texte seul : consulter la météo, lire un agenda, faire un calcul, envoyer un e-mail. Pour s'en servir, l'agent remplit les détails dont l'outil a besoin (une ville et un jour, pour une prévision météo) et lit ce qui revient.

### Outils qui lisent, outils qui changent les choses

Certains outils ne font que lire : ils cherchent une information et ne changent rien. D'autres écrivent : ils changent quelque chose dans le monde, comme envoyer un e-mail ou acheter un billet. Une erreur avec un outil de lecture coûte un tour. Une erreur avec un outil d'écriture peut coûter bien plus, c'est pourquoi les agents prudents vous demandent avant d'en utiliser un.

### L'étiquette est tout ce que voit le modèle

Le modèle ne voit jamais l'intérieur d'un outil. Pour chaque outil, il reçoit une étiquette : un nom, une courte description de ce que fait l'outil, et les détails dont il a besoin. Il choisit en comparant votre demande à ces étiquettes.

Les mots de l'étiquette comptent donc autant que l'outil lui-même. Une étiquette claire mène au bon choix. Une étiquette floue mène à des erreurs, même quand le bon outil est juste là.

### Moins d'outils, moins d'erreurs

Ne donnez à un agent que les outils dont une tâche a besoin. Moins de choix, c'est moins de mauvais choix, et un agent ne peut pas mal utiliser un outil qu'il n'a pas.

Ci-dessous, vous jouez l'assistant de l'agent : tendez-lui le bon outil pour trois demandes. Puis changez une étiquette et voyez ce que l'agent en fait.

## Essayez : Choisir le bon outil

Tendez à l'agent le bon outil pour trois demandes ; choisissez un outil pour lire son étiquette. Puis laissez l'agent choisir d'après les étiquettes, rendez floue l'étiquette d'E-mail et renvoyez la demande.

https://agenticademy.org/fr/paths/agents-for-everyone/03-tools

## Vérification

### Question 1 sur 3

Que sait le modèle d'un outil quand il décide de l'utiliser ?

- Comment le code de l'outil fonctionne à l'intérieur.
- **✓** Son nom, sa description et les détails dont il a besoin.
- Tout ce que l'outil a jamais renvoyé.

**Pourquoi:** L'étiquette est tout ce que reçoit le modèle. C'est pourquoi une description claire compte autant que l'outil lui-même.

### Question 2 sur 3

Lequel de ces outils est un outil d'écriture, qui change quelque chose ?

- Consulter la météo de demain.
- Chercher un restaurant.
- **✓** Envoyer un e-mail.

**Pourquoi:** Envoyer un e-mail change quelque chose hors de la conversation et ne peut pas être annulé. Consulter la météo ou chercher un restaurant ne fait que lire.

### Question 3 sur 3

Pourquoi ne donner à un agent que les outils dont une tâche a besoin ?

- Cela rend l'agent plus lent mais moins cher.
- **✓** Moins de choix, c'est moins de mauvais choix, et moins de dégâts si cela arrive.
- Les agents ne peuvent avoir que trois outils.

**Pourquoi:** Un agent ne peut pas mal utiliser un outil qu'il n'a pas. Moins d'outils, bien étiquetés, rendent les bons choix plus faciles et les mauvais moins coûteux.

## Ce que vous savez faire maintenant

- Expliquer ce qu'est un outil et distinguer les outils de lecture des outils d'écriture.
- Expliquer pourquoi la description d'un outil oriente les choix de l'agent.
- Défendre l'idée de ne donner à un agent que les outils dont il a besoin.

---

[Retour à Les agents pour tous](README.md) · [Suivant : Mémoire et contexte](04-memory-and-context.md)
