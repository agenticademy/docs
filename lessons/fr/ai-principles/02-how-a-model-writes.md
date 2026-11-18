# Comment un modèle écrit

Principes de l'IA · Chapitre 2 sur 8 · 14 minutes

Un modèle de langage écrit un token à la fois en le tirant au sort dans une liste de probabilités, la température change la façon de tirer, et rien dans cette étape ne vérifie si les mots sont vrais.

## Objectifs

- Lire les probabilités pour un prompt et laisser le modèle prendre le mot le plus probable.
- Choisir vous-même les cinq mots suivants.
- Écrire à basse et à haute température et voir le texte dériver.
- Réussir la vérification.

## La lecture

### Un morceau à la fois

Au chapitre précédent, un classifieur a appris à partir d'exemples à ranger des points par catégorie. Un modèle de langage, le type d'IA derrière les assistants conversationnels, a appris d'une immense quantité de texte à faire une seule chose : à partir d'un texte, dire ce qui a des chances de venir ensuite. Il écrit en répétant cette étape. Il calcule une probabilité pour chaque morceau suivant possible, en choisit un, l'ajoute au texte et recommence.

### Tokens et probabilités

Ces morceaux s'appellent des tokens. Un token est un court morceau de texte : un mot, une partie de mot ou un signe de ponctuation. Une probabilité est un nombre de 0 % à 100 %, et les probabilités de tous les tokens suivants possibles font 100 % au total. Après « Le café ouvre à 8 h du », le token « matin » reçoit une probabilité élevée et « lune » une probabilité infime. Le texte de départ s'appelle le prompt.

### Température

Le modèle tire le token suivant au sort, en pondérant par les probabilités, si bien que les tokens probables sortent le plus souvent. La température est un réglage qui remodèle les probabilités avant le tirage. Près de 0, le token le plus probable gagne presque toujours, et le même prompt donne presque le même texte à chaque fois. Plus haut, les tokens peu probables sont choisis plus souvent : le texte varie davantage et, au-delà d'un certain point, dérive vers le non-sens.

### Fluide ne veut pas dire vrai

Chaque token est choisi parce qu'il a des chances de venir ensuite, pas parce qu'il est vrai. Rien dans cette étape ne vérifie un fait. Un modèle peut écrire une phrase fluide avec une date, un prix ou un nom faux, et une basse température n'y change rien : elle ne fait que choisir les mots les plus probables. Vérifier ce qu'il écrit reste votre travail, et un chapitre suivant montre comment.

### Le modèle de cette page

L'explorateur ci-dessous fait tourner un tout petit modèle que votre navigateur entraîne à l'ouverture de la page, sur dix fables d'Ésope. Ses tokens sont des mots entiers et des signes de ponctuation. Il ne regarde que les deux derniers et compte ce qui les suivait dans les fables. Les modèles derrière les assistants conversationnels utilisent des tokens qui peuvent être des parties de mots, prennent en compte bien plus de texte et ont des milliards de paramètres, les nombres qu'ils ajustent en apprenant. L'étape du choix fonctionne de la même façon.

## Essayez : Explorateur du mot suivant

Lisez les probabilités pour un prompt, choisissez des mots vous-même, puis déplacez la température et regardez le texte dériver.

https://agenticademy.org/fr/paths/ai-principles/02-how-a-model-writes

## Vérification

### Question 1 sur 3

Vous tapez « Notre train pour la côte part à » et un modèle continue le texte. Que fait-il ?

- Il consulte les horaires et recopie l'heure.
- **✓** Il donne une probabilité à chaque token suivant possible, en tire un au sort, l'ajoute, puis recommence.
- Il écrit toute la réponse d'un coup, puis la vérifie par rapport aux faits.
- Il choisit toujours le token le plus surprenant, pour garder le texte intéressant.

**Pourquoi:** Un modèle de langage écrit un token à la fois, en tirant chacun au sort selon des probabilités. Il ne consulte rien sauf s'il est relié à un outil, il n'écrit pas tout d'un coup et ne vérifie pas les faits, et ce sont les tokens probables qui sont choisis le plus souvent, pas les surprenants.

### Question 2 sur 3

Vous posez deux fois la même question à un modèle avec une haute température. À quoi devez-vous vous attendre ?

- Exactement la même réponse les deux fois.
- **✓** Des réponses qui peuvent différer, car chaque token est tiré au sort selon des probabilités remodelées.
- Une erreur, car une haute température n'est pas autorisée.
- Des réponses plus exactes, car le modèle essaie plus d'options.

**Pourquoi:** Une haute température égalise les probabilités, donc chaque tirage peut partir dans une autre direction et deux réponses peuvent différer. Près de 0, elles seraient presque identiques. Une haute température est un réglage autorisé, et elle ne rend pas les réponses plus exactes : elle rend seulement les tokens peu probables plus fréquents.

### Question 3 sur 3

Un modèle écrit, avec aisance et sans réserve : « Le café d'Elm Street ouvre à 7 h 30 le dimanche. » Que vous dit cette formulation fluide sur l'horaire ?

- Il est juste, sinon le modèle ne l'écrirait pas avec autant d'aisance.
- Il est juste si la température était basse.
- **✓** Rien de sûr : les mots ont été choisis parce qu'ils étaient probables, alors vérifiez l'horaire auprès du café.
- Il est faux, car les modèles inventent toujours les horaires.

**Pourquoi:** Une formulation fluide montre que les tokens étaient probables, pas qu'ils sont vrais. Une basse température fait seulement choisir au modèle les tokens les plus probables, qui peuvent quand même être faux, et les modèles ne se trompent pas toujours non plus. Vérifiez les faits comme les horaires d'ouverture auprès d'une source fiable.

## Ce que vous savez faire maintenant

- Expliquer qu'un modèle de langage écrit un token à la fois, en tirant chacun au sort selon des probabilités.
- Dire ce que la température change, et ce qu'elle ne change pas.
- Expliquer pourquoi une réponse fluide et assurée peut quand même être fausse.

---

[Retour à Principes de l'IA](README.md) · [Suivant : Fort ici, faible là](03-strong-here-weak-there.md)
