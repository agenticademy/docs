# Ce qu'est l'IA, et ce qu'elle n'est pas

Principes de l'IA · Chapitre 1 sur 8 · 12 minutes

La plupart des systèmes d'IA utilisés aujourd'hui apprennent des motifs à partir d'exemples et s'en servent pour inférer un résultat pour quelque chose de nouveau. Un tel système n'est ni un esprit, ni une base de données, ni un moteur de recherche, et ses exemples façonnent chaque réponse.

## Objectifs

- Placer des exemples de deux catégories et laisser le classifieur les apprendre.
- Tester un nouveau fruit et voir quels exemples décident.
- Ajouter un lot biaisé d'exemples et regarder la frontière se déplacer.
- Réussir la vérification.

## La lecture

### Apprendre à partir d'exemples

La plupart des systèmes d'IA utilisés aujourd'hui sont des programmes qui apprennent des motifs à partir d'exemples, puis utilisent ces motifs pour produire un résultat pour quelque chose de nouveau. Les exemples s'appellent des données. Le résultat peut être une étiquette comme « spam », un nombre comme les ventes de la semaine prochaine, ou le mot suivant d'une phrase.

Obtenir un résultat de cette façon s'appelle l'inférence : la réponse vient des motifs, et non d'une recherche dans une liste.

### L'entraînement, puis l'utilisation

Un tel système a deux phases. Pendant l'entraînement, on lui montre de nombreux exemples et il s'ajuste pour leur correspondre. Pendant l'utilisation, il applique ce qu'il a appris à des entrées qu'il n'a jamais vues. L'utiliser ne change pas ce qu'il a appris ; pour cela, il faut un nouveau cycle d'entraînement.

### Ce qu'il n'est pas

Ce n'est pas un esprit : il calcule des résultats à partir de motifs et ne veut rien. Ce n'est pas une base de données : une base de données ne rend que ce qui y a été stocké, alors qu'un système d'IA répond pour des entrées que personne n'a stockées, et peut se tromper. Ce n'est pas un moteur de recherche : il ne va pas chercher de pages, même si certains outils le relient à un moteur de recherche.

### Les exemples décident

Un système n'apprend que de ses exemples, donc ils façonnent chaque réponse. Si les exemples sont biaisés, c'est-à-dire qu'une catégorie ou une source en compte bien plus que les autres, les réponses penchent du même côté. Un système qui a surtout appris à partir de photos prises dans un seul pays a tendance à moins bien réussir sur des photos d'ailleurs.

### Dans le visuel

Vous entraînez un classifieur, un programme qui range des choses par catégorie, à distinguer les citrons des oranges selon leur taille et leur couleur. Chaque fruit est un point sur une carte. Pour juger un nouveau fruit, il trouve les 3 exemples les plus proches de lui sur la carte, ses plus proches voisins, et répond avec la catégorie de la majorité d'entre eux. La ligne où sa réponse passe d'une catégorie à l'autre est sa frontière.

Pour ce classifieur simple, l'entraînement consiste à garder les exemples. Les grands modèles, eux, ajustent un très grand nombre de valeurs internes pendant leur entraînement, et gardent ces valeurs plutôt que les exemples.

## Essayez : Entraînez un tout petit classifieur

Placez des citrons et des oranges sur la carte, testez un fruit qu'il n'a pas vu, puis ajoutez un lot biaisé et regardez la frontière se déplacer.

https://agenticademy.org/fr/paths/ai-principles/01-what-ai-is

## Vérification

### Question 1 sur 3

Un système a été entraîné sur des milliers de photos étiquetées « chat » ou « chien ». Maintenant, il étiquette une photo qu'il n'a jamais vue. Que fait-il ?

- Il cherche la photo dans une réserve de réponses.
- **✓** Il infère une étiquette à partir de motifs dans ses exemples.
- Il cherche la même photo sur le web.
- Il réfléchit à la photo comme le ferait une personne.

**Pourquoi:** Il applique des motifs tirés de ses exemples étiquetés à une nouvelle entrée : c'est l'inférence. Une réserve de réponses ne pourrait rendre que des photos qu'elle avait déjà, une recherche sur le web trouve des pages qui existent, et le système a des motifs, pas les pensées d'une personne.

### Question 2 sur 3

Un outil qui trie la boîte de réception d'un support client a appris à partir de nombreux messages de facturation et de quelques messages de livraison seulement. Un nouveau message n'est pas clair : il pourrait être l'un ou l'autre. Quel est le résultat probable ?

- **✓** Il a plus de chances de le classer comme message de facturation.
- Il a autant de chances de le classer dans l'une ou l'autre catégorie, parce qu'il a vu les deux.
- Il refuse de trier le message.

**Pourquoi:** Ses exemples penchent vers la facturation, donc près de la frontière ses réponses penchent aussi de ce côté, comme le lot d'oranges a attiré la frontière dans le visuel. Il a vu des messages de livraison, mais trop peu pour tenir leur place. Et un classifieur comme celui du visuel choisit toujours une de ses catégories : il n'a aucun moyen de refuser.

### Question 3 sur 3

Dans le visuel, pourquoi le classifieur s'est-il mis à appeler orange un citron typique ?

- Le citron a changé de taille et de couleur.
- **✓** Le lot a placé des oranges tout près de lui, si bien que la plupart de ses plus proches voisins sont devenus des oranges.
- Quelqu'un a dit au classifieur que la bonne réponse était orange.
- Plus d'exemples rendent toujours un classifieur moins précis.

**Pourquoi:** Le citron et la règle (les 3 plus proches décident) sont restés les mêmes ; seuls les exemples ont changé. Personne ne lui a donné la réponse. Avoir plus d'exemples n'est pas le problème en soi : le problème, ce sont des exemples déséquilibrés.

## Ce que vous savez faire maintenant

- Expliquer avec des mots simples qu'un système d'IA infère des résultats à partir de motifs dans ses exemples.
- Distinguer l'entraînement (quand un système apprend de ses exemples) de l'utilisation (quand il applique ce qu'il a appris).
- Dire pourquoi un système d'IA n'est ni un esprit, ni une base de données, ni un moteur de recherche.
- Expliquer pourquoi des exemples biaisés mènent à des réponses biaisées.

---

[Retour à Principes de l'IA](README.md) · [Suivant : Comment un modèle écrit](02-how-a-model-writes.md)
