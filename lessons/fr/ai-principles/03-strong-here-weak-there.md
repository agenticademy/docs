# Fort ici, faible là

Principes de l'IA · Chapitre 3 sur 8 · 15 minutes

L'IA réussit certaines tâches difficiles et échoue sur d'autres qui semblent faciles, elle peut affirmer des choses fausses avec assurance, et elle est plus utile quand elle dit qu'elle ne sait pas.

## Objectifs

- Répondre à six questions ou dire « Je ne sais pas », puis les noter de deux façons.
- Placer six tâches du quotidien sur une carte datée qui montre où l'IA réussit et où elle échoue.
- Réussir la vérification.

## La lecture

### Inégale, et pas selon la difficulté

Au chapitre 2, vous avez vu qu'un modèle écrit en choisissant les morceaux de texte suivants les plus probables, appris à partir des exemples sur lesquels il a été entraîné. Ce qu'il réussit dépend de ces exemples, pas de ce que les gens trouvent difficile. Il peut traduire un panneau en trois langues en quelques secondes, puis se tromper en comptant les lettres d'un mot, parce qu'il lit des morceaux de mots plutôt que des lettres une à une.

Les chercheurs appellent cela une frontière en dents de scie : la limite entre ce que l'IA fait bien et ce qu'elle fait mal zigzague. L'expression vient d'une étude de 2023 dans laquelle des consultants ont utilisé l'IA sur des tâches réalistes (Dell'Acqua et ses collègues). Sur les tâches à l'intérieur de la frontière, leur travail s'est amélioré. Sur une tâche juste à l'extérieur, ceux qui utilisaient l'IA avaient raison moins souvent que ceux qui travaillaient sans elle. La frontière bouge aussi à mesure que les modèles changent, donc toute carte de cette frontière a besoin d'une date.

### Sûr de lui, et faux

Un modèle produit toujours une réponse, et ses mots semblent tout aussi sûrs quand il se trompe. Écrire quelque chose de faux sur un ton assuré, comme une citation d'un livre qui n'existe pas, s'appelle une confabulation ; beaucoup de gens parlent d'hallucination. Être calibré signifie que votre confiance correspond à la fréquence à laquelle vous avez raison : quand vous dites que vous êtes sûr, vous l'êtes presque toujours.

### Pourquoi deviner paie à la plupart des tests

S'abstenir, c'est dire « Je ne sais pas » au lieu de répondre. Que ce soit payant dépend du barème, la façon dont un test transforme les réponses en points. Si seules les bonnes réponses comptent, deviner ne peut qu'aider, donc quelqu'un qui répond toujours bat quelqu'un qui avoue ses doutes. Un article de 2025 de chercheurs d'OpenAI soutient que la plupart des tests utilisés pour classer les modèles d'IA notent de cette façon, si bien qu'un modèle qui devine se classe devant un modèle qui admet ne pas être sûr. La solution qu'il propose : qu'une mauvaise réponse coûte plus cher que « Je ne sais pas ».

### Dans le visuel

Vous répondez à six questions ; personne ne pourrait connaître certaines réponses à partir de cette page. Vos réponses sont notées des deux façons, à côté de deux joueurs simulés. Ensuite, vous placez six tâches du quotidien sur une carte datée : en général ça va, vérifier de près, ou ne pas s'y fier.

## Essayez : Répondre ou s'abstenir, puis placer les tâches

Répondez à six questions, voyez-les notées de deux façons, puis placez six tâches du quotidien sur une carte datée.

https://agenticademy.org/fr/paths/ai-principles/03-strong-here-weak-there

## Vérification

### Question 1 sur 3

Un quiz donne 1 point pour une bonne réponse, 0 pour « Je ne sais pas » et moins 1 pour une mauvaise réponse. Vous êtes sûr de votre réponse à environ 30 %. Qu'est-ce qui rapporte le plus en moyenne ?

- Répondre quand même : une supposition vaut le coup avec n'importe quel barème.
- **✓** Dire « Je ne sais pas » : avec 30 % de certitude, deviner fait perdre des points en moyenne.
- L'un ou l'autre : les deux rapportent autant en moyenne.
- Répondre, et ajouter une explication plus longue pour gagner plus.

**Pourquoi:** Répondre rapporte 0,3 × 1 + 0,7 × (−1) = −0,4 point en moyenne et « Je ne sais pas » rapporte 0, donc les deux ne se valent pas et s'abstenir rapporte le plus. Deviner ne vaut le coup que si les mauvaises réponses ne coûtent rien, et une explication plus longue ne change pas les points. Il en va de même pour un modèle noté ainsi.

### Question 2 sur 3

Un assistant conversationnel traduit bien une longue lettre en japonais, puis se trompe sur le nombre de lettres d'un mot court. Qu'est-ce que cela montre ?

- Il est cassé, donc rien de ce qu'il produit n'est fiable.
- Compter des lettres est plus difficile que traduire, pour les gens aussi.
- Il ne s'est pas appliqué sur la tâche facile, comme une personne qui s'ennuie.
- **✓** Ce qu'il réussit ne suit pas ce que les gens trouvent difficile.

**Pourquoi:** C'est la frontière en dents de scie : un modèle peut bien faire une tâche difficile pour les gens et rater une tâche à la portée d'un enfant, parce qu'il lit des morceaux de mots, pas des lettres. Jugez donc chaque type de tâche séparément. Une erreur ne rend pas tout son travail indigne de confiance, compter n'est pas plus difficile pour les gens, et il ne se relâchait pas : l'erreur vient de la façon dont il lit le texte.

### Question 3 sur 3

Vous demandez à un assistant conversationnel sans recherche web si la boulangerie de votre rue est ouverte en ce moment. Il répond : « Oui, elle est ouverte jusqu'à 18 h aujourd'hui. » Que devriez-vous faire ?

- **✓** Le traiter comme une supposition et consulter la page de la boulangerie elle-même, ou appeler.
- Lui faire confiance : il a l'air sûr, donc il doit savoir.
- Redemander : s'il dit la même chose deux fois, c'est juste.
- Lui faire confiance, parce que la réponse est courte et précise.

**Pourquoi:** Il n'a aucun moyen de voir les horaires du jour, donc une réponse sûre et précise peut quand même être une confabulation : les mots d'un modèle sonnent pareil, qu'il sache ou non. Redemander ne donne qu'une autre supposition, qui peut correspondre ou non. Consultez une source qui sait.

## Ce que vous savez faire maintenant

- Expliquer pourquoi l'IA peut bien faire une tâche difficile et échouer sur une tâche qui semble facile.
- Dire pourquoi une réponse assurée peut quand même être fausse, et pourquoi ne noter que les bonnes réponses récompense le fait de deviner.
- Pour une tâche du quotidien, décider si le résultat de l'IA est en général correct, demande une vérification ou ne mérite pas qu'on s'y fie.

---

[Retour à Principes de l'IA](README.md) · [Suivant : Quoi confier](04-what-to-hand-over.md)
