# Comparer des modèles

Comprendre les modèles · Chapitre 7 sur 8 · 15 minutes

Les benchmarks et les classements aident à comparer des modèles, mais les marges d'erreur, les réglages d'effort, le coût, les tests divulgués, les tests saturés, le programme du test et qui l'a lancé changent tous le sens d'un score.

## Objectifs

- Lire un classement avec ses intervalles et trouver les égalités.
- Comparer un modèle à cinq réglages d'effort avec un passage à un autre modèle.
- Associer quatre pièges des benchmarks à des exemples.
- Réussir la vérification.

## La lecture

### Benchmarks et classements

Un benchmark est un ensemble fixe de tâches de test avec une façon de noter les réponses. Un classement range les modèles selon ces scores. Le visuel en utilise deux réels. Sur Arena, des personnes envoient un prompt, lisent deux réponses de modèles qu'elles ne voient pas et votent pour celle qu'elles préfèrent ; des millions de votes deviennent un score pour chaque modèle. Artificial Analysis, une entreprise indépendante, lance elle-même neuf benchmarks et les combine en un seul nombre, son Intelligence Index.

### Chaque score a une marge

Un score tiré de votes, ou d'un échantillon de tâches, est une estimation. Arena l'écrit comme un score ± une marge : le vrai score se trouve très probablement dans cette plage, appelée intervalle à 95 %. Moins de votes donnent une plage plus large. Quand les plages de deux modèles se chevauchent, le classement ne peut pas les départager : considérez-les comme à égalité, quel que soit leur rang.

### Réglages et coût

Beaucoup de modèles ont un réglage d'effort, comme faible, moyen, élevé ou maximal : combien le modèle peut réfléchir avant de répondre, comme au chapitre 3. Plus d'effort donne souvent un meilleur score, mais le modèle écrit plus de tokens, donc chaque tâche coûte plus cher et prend plus de temps. Artificial Analysis montre aussi un coût par tâche : ce qu'a coûté l'exécution du modèle sur l'une de ses tâches, en dollars américains. Un score veut dire peu de chose sans son réglage et son coût.

### Pièges

Contamination : les questions et les réponses du test ont fuité dans les données d'entraînement, donc un modèle peut s'en souvenir au lieu de les résoudre. Saturation : les meilleurs modèles obtiennent tous presque le maximum, donc le test ne les départage plus. Le harness est le programme qui fait passer le test, avec ses instructions, ses outils et ses nouvelles tentatives ; un meilleur harness peut faire monter le score du même modèle. Le score d'un créateur est obtenu à sa façon et peut différer d'un score indépendant. Dérive de version : les tests changent d'une version à l'autre, donc un score a besoin de sa version et de sa date.

### Dans le visuel

Les chiffres d'Arena et d'Artificial Analysis sont réels, tels que publiés, dans un instantané daté du 2026-11-03. L'axe des coûts du graphique est logarithmique : de 0,10 $US à 1 $US prend la même largeur que de 1 $US à 10 $US, donc les modèles bon marché et les modèles chers tiennent tous. Les exemples de la dernière étape sont inventés.

## Essayez : Lire les scores des benchmarks

Trouvez les égalités dans un vrai classement, montez l'échelle d'effort d'un modèle, puis associez quatre pièges à des exemples inventés.

https://agenticademy.org/fr/paths/understanding-models/07-comparing-models

## Vérification

### Question 1 sur 3

Dans un exemple, le modèle A obtient 1 310 ± 8 dans un classement et le modèle B 1 302 ± 9. Que pouvez-vous dire ?

- A est nettement meilleur que B.
- **✓** Leurs intervalles se chevauchent, donc le classement ne peut pas les départager.
- B est meilleur, parce que son intervalle est plus large.
- Les scores doivent être faux.

**Pourquoi:** L'intervalle de A va de 1 302 à 1 318, et celui de B de 1 293 à 1 311. Les plages se chevauchent, donc l'écart peut n'être que du bruit : considérez-les comme à égalité, et non A comme nettement meilleur. Un intervalle plus large signifie moins de certitude, par exemple moins de votes, pas un meilleur modèle. Et rien ne dit que les scores sont faux : ce sont des estimations avec leurs marges.

### Question 2 sur 3

Un rapport dit que le modèle X obtient 5 points de plus que le modèle Y sur un index. Que devriez-vous demander avant de passer à X ?

- Rien : 5 points, c'est une victoire nette.
- **✓** À quel réglage d'effort chacun a tourné, et ce que chacun a coûté par tâche.
- Lequel des deux créateurs est le plus grand.
- Quel modèle est sorti en premier.

**Pourquoi:** Dans le visuel, passer de faible à maximal a fait bouger chaque modèle à cinq réglages de 20 à 29 points, pour environ 8 à 20 fois le coût par tâche. Un écart de 5 points peut donc venir des seuls réglages, et le meilleur score peut coûter bien plus cher par tâche. La taille du créateur et la date de sortie ne disent rien ni de l'un ni de l'autre.

### Question 3 sur 3

Un article de blog met le score du modèle A sur la version actuelle d'un test à côté du score du modèle B sur la version d'il y a un an. Le nombre de A est plus élevé. Que pouvez-vous en conclure ?

- A est meilleur, parce que son nombre est plus élevé.
- B est meilleur, parce qu'il a été testé en premier.
- **✓** Pas grand-chose pour l'instant : des versions différentes utilisent des tâches différentes, donc les deux nombres ne sont pas sur la même échelle.
- La nouvelle version doit être plus facile.

**Pourquoi:** C'est la dérive de version. Une nouvelle version change les tâches, leur nombre ou leur pondération, donc ses scores ne sont pas sur l'ancienne échelle. Le 2026-11-03, par exemple, l'index d'Artificial Analysis était à la version 5.0.1, avec neuf évaluations. Comparez des modèles sur la même version, consultée à la même date. Être testé en premier ne rend pas un modèle meilleur, et rien ne dit que la nouvelle version est plus facile.

## Ce que vous savez faire maintenant

- Lire les intervalles d'un classement et dire quels modèles il ne peut pas départager.
- Comparer des modèles à des réglages d'effort nommés, avec leur coût par tâche.
- Repérer dans une affirmation la contamination, la saturation, les effets du harness et les scores propres d'un créateur.
- Demander la version, la date et qui a fait passer le test avant de se fier à un score.

---

[Retour à Comprendre les modèles](README.md) · [Suivant : Choisir un modèle pour un agent](08-choosing-for-an-agent.md)
