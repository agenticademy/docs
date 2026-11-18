# Mémoire et contexte

Les agents pour tous · Chapitre 4 sur 8 · 12 minutes

Ce qu'un agent peut voir à chaque étape est limité, donc ce qu'il garde, abandonne et note décide de ce qu'il sait.

## Objectifs

- Remplir le contexte jusqu'à ce que les premiers messages en sortent.
- Enregistrer le bon fait pour que l'agent réponde correctement plus tard.
- Réussir la vérification.

## La lecture

### Ce que lit le modèle

Chaque fois que le modèle décide d'une étape, il lit un bloc de texte appelé le contexte : votre demande, la conversation jusque-là et ce que les outils ont renvoyé. C'est tout ce dont il dispose. Si quelque chose n'est pas dans le contexte, le modèle ne le sait pas pour cette étape.

### Un bureau à la place limitée

Le contexte a une taille limite, comptée en tokens. Un token est un morceau de mot : un mot court fait environ un token, et en anglais un token fait environ quatre caractères. Les limites varient selon les modèles, de quelques milliers de tokens à un million ou plus, mais il y en a toujours une.

Imaginez le contexte comme un bureau. On ne peut y poser qu'une certaine quantité de papier à la fois.

### Quand le bureau est plein

Une longue conversation remplit le bureau. Alors quelque chose doit partir : les messages les plus anciens sont retirés, ou plusieurs sont remplacés par un court résumé. Ce qui est retiré, l'agent ne le sait plus, même si vous le lui aviez dit plus tôt.

### La mémoire : un carnet à côté du bureau

La mémoire est autre chose. Ce sont des notes que l'agent écrit quelque part hors du contexte, comme un fichier, et qu'il relit quand il en a besoin. Une note survit quand le bureau est débarrassé, mais seulement si l'agent l'a enregistrée, et seulement la partie qu'il a choisi de noter.

### Plus n'est pas toujours mieux

Un contexte encombré prend plus de temps à lire, coûte plus cher et peut noyer le seul fait qui compte. Les bons agents gardent sur le bureau ce dont l'étape suivante a besoin, et écrivent le reste dans le carnet.

## Essayez : Un bureau à la place limitée

Remplissez le bureau jusqu'à ce que les messages les plus anciens en sortent, et voyez ce que l'agent oublie. Puis recommencez, enregistrez un fait dans le carnet et posez de nouveau la question.

https://agenticademy.org/fr/paths/agents-for-everyone/04-memory-and-context

## Vérification

### Question 1 sur 3

Qu'y a-t-il dans le contexte d'un agent pour une étape ?

- Tout internet.
- **✓** La demande, la conversation jusque-là et les résultats d'outils qui tiennent encore.
- Seulement votre dernier message.

**Pourquoi:** Le modèle ne lit que ce qu'on place devant lui pour cette étape, jusqu'à sa taille limite.

### Question 2 sur 3

Le contexte est plein et un nouveau message arrive. Que se passe-t-il en général ?

- L'agent s'arrête pour de bon.
- **✓** Les messages plus anciens sont retirés ou résumés pour faire de la place.
- Le modèle se dote d'un contexte plus grand.

**Pourquoi:** La limite est fixe pour un modèle, donc l'agent doit faire de la place. Ce qu'il retire, il ne le sait plus, sauf s'il a enregistré une note.

### Question 3 sur 3

En quoi la mémoire diffère-t-elle du contexte ?

- **✓** La mémoire, ce sont des notes gardées hors du contexte et relues quand il le faut.
- C'est la même chose.
- La mémoire est ce que le modèle a appris à l'entraînement.

**Pourquoi:** La mémoire survit quand le contexte est vidé, parce qu'elle se trouve ailleurs. L'agent doit choisir de l'enregistrer et de la relire.

## Ce que vous savez faire maintenant

- Expliquer ce qu'est un contexte et pourquoi il a une limite.
- Dire ce qui se passe quand il est plein.
- Distinguer le contexte de la mémoire, et dire pourquoi il compte d'enregistrer les bons faits.

---

[Retour à Les agents pour tous](README.md) · [Suivant : Les plans, et les agents qui travaillent ensemble](05-plans-and-teams.md)
