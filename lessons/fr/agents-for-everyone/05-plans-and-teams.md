# Les plans, et les agents qui travaillent ensemble

Les agents pour tous · Chapitre 5 sur 8 · 14 minutes

Les grandes tâches sont découpées en étapes, et parfois partagées entre plusieurs agents menés par un seul.

## Objectifs

- Choisir les étapes qui peuvent se faire en même temps.
- Exécuter la tâche avec un seul agent, puis avec une équipe.
- Réussir la vérification.

## La lecture

### Un plan est une liste d'étapes

Donnez à un agent un grand objectif, comme organiser un déjeuner d'équipe, et il commence par faire un plan : il découpe l'objectif en étapes plus petites. Certaines étapes dépendent d'autres. Vous ne pouvez pas réserver un restaurant avant de connaître le jour. D'autres étapes sont indépendantes et peuvent se faire en même temps. Des étapes qui se font en même temps sont dites en parallèle.

### Un agent, ou une équipe

Un seul agent peut suivre tout le plan par lui-même, une étape après l'autre. Tout ce qu'il lit en chemin, chaque réponse et chaque résultat, s'accumule dans son contexte, le texte que le modèle lit à chaque étape (chapitre 4). Une longue tâche peut le remplir.

Une autre façon de faire est une équipe d'agents. Un agent principal, appelé orchestrateur, garde le plan et confie chaque partie à un spécialiste : un autre agent avec ses propres instructions, ses propres outils et son propre contexte. Transmettre une partie du travail d'un agent à un autre s'appelle un passage de relais. Le spécialiste fait sa partie et renvoie un court résultat. L'orchestrateur ne lit jamais les détails, seulement les résumés.

### Ce que coûte une équipe

Une équipe peut finir plus tôt, parce que les étapes indépendantes se font en parallèle, et le contexte de chaque agent reste petit et centré sur une seule chose. Mais chaque agent fait ses propres appels au modèle, donc une équipe coûte en général plus cher. Et chaque passage de relais est un endroit de plus où un détail peut se perdre ou être mal compris.

Une équipe vaut la peine quand une tâche a de grandes parties qui ne dépendent pas les unes des autres. Pour une petite tâche, un seul agent est plus simple et moins cher.

### Les mots de ce chapitre

Plan : la liste des étapes vers un objectif. En parallèle : en même temps. Orchestrateur : l'agent principal qui répartit le travail et combine les résultats. Spécialiste : un agent chargé d'une partie du travail. Passage de relais : transmettre une partie du travail, avec une courte consigne, d'un agent à un autre.

## Essayez : Un agent ou une équipe

Marquez les étapes qui peuvent commencer tout de suite, en même temps. Puis exécutez la même tâche avec un seul agent et avec une équipe, et comparez les bilans.

https://agenticademy.org/fr/paths/agents-for-everyone/05-plans-and-teams

## Vérification

### Question 1 sur 3

Que fait un agent orchestrateur ?

- Il fait chaque étape lui-même.
- **✓** Il répartit le travail, confie des parties à des agents spécialistes et combine leurs résultats.
- Il vérifie qu'internet fonctionne.

**Pourquoi:** Un orchestrateur mène : il planifie, confie des parties du travail à des spécialistes et rassemble leurs résultats. Un seul agent qui fait chaque étape lui-même est l'autre façon de travailler.

### Question 2 sur 3

Quand une équipe d'agents aide-t-elle le plus ?

- **✓** Quand la tâche a de grandes parties qui ne dépendent pas les unes des autres.
- Pour une question en une étape.
- Toujours : plus d'agents, c'est toujours mieux.

**Pourquoi:** Les parties indépendantes peuvent se faire en parallèle, chacune avec un contexte petit et ciblé. Une tâche en une étape ne gagne rien à une équipe et paie quand même les passages de relais.

### Question 3 sur 3

Lequel est un vrai coût de l'usage de plusieurs agents ?

- Ils ne peuvent pas utiliser d'outils.
- **✓** Plus d'appels au modèle, et plus de passages de relais où quelque chose peut mal tourner.
- Ils oublient l'objectif par conception.

**Pourquoi:** Chaque agent fait ses propres appels au modèle, donc la facture grossit, et chaque passage de relais peut perdre ou déformer un détail. Dans le bilan, l'équipe a fait plus de deux fois plus d'appels.

## Ce que vous savez faire maintenant

- Expliquer ce qu'est un plan et pourquoi certaines étapes peuvent se faire en parallèle.
- Décrire un orchestrateur et ses agents spécialistes.
- Comparer un seul agent et une équipe pour une tâche donnée.

---

[Retour à Les agents pour tous](README.md) · [Suivant : Les standards, en mots simples](06-standards.md)
