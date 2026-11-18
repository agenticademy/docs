# Dire ce que vous voulez

Principes de l'IA · Chapitre 5 sur 8 · 15 minutes

Là où une demande laisse un vide, le modèle le comble souvent par une supposition probable ; une bonne demande décrit donc le résultat, la façon de travailler et le comportement que vous voulez, avec les faits et un exemple que vous seul pouvez donner.

## Objectifs

- Lire une demande vague et la réponse obtenue, et marquer ce que la réponse a dû supposer.
- Ajouter des éléments à la demande un par un jusqu'à ce que le vérificateur ne trouve plus de vide.
- Comparer les réponses à la demande vague et à la demande complète.
- Réussir le test.

## La lecture

### Un vide appelle une supposition

Le chapitre 4 portait sur ce qu'on peut confier. Celui-ci porte sur la façon de demander. Ce que vous tapez dans un assistant conversationnel d'IA est un prompt : le texte dont part le modèle, comme au chapitre 2. Dans un simple chat, sans accès à vos fichiers ni à votre agenda, tout ce que le modèle sait de votre situation, c'est ce que vous écrivez.

Là où votre demande laisse un vide, le modèle le comble souvent par ce qui est probable en général, comme il choisit des mots probables. Le résultat se lit bien, si bien que les suppositions passent facilement inaperçues : une date que personne n'a fixée, une promesse que personne n'a faite.

### Trois choses à décrire

Rédigez votre demande comme un brief : les consignes que vous donneriez à un assistant compétent qui ne sait rien de votre situation. Trois choses méritent d'être décrites.

Le résultat : à qui il s'adresse, sa forme (une liste, une affiche, un e-mail) et sa longueur.

La façon de travailler : les étapes à suivre, et ce qu'il faut vous demander avant de commencer. « S'il manque un fait, demandez-moi » transforme une supposition en question.

Le comportement : le ton à adopter, et ce qu'il faut éviter, comme des promesses que vous ne pouvez pas tenir.

### Ce que vous seul savez

Deux autres éléments apportent ce que le modèle ne peut pas deviner. Le contexte, ce sont les faits de fond : dates, noms, ce qui est déjà décidé. Un exemple montre à quoi ressemble un bon résultat, comme une affiche qui a bien marché la dernière fois. Dites ce qu'il faut en reprendre, par exemple sa mise en page, sinon le modèle risque de recopier aussi son ancienne date.

Des formules comme « fais-le bien » ajoutent de la longueur, pas de l'information : elles ne disent pas ce que bien veut dire.

### Dans le visuel

Vous partez d'une demande vague pour une affiche de jardin partagé et de la réponse obtenue, et vous marquez ce que cette réponse a dû supposer. Ensuite, vous ajoutez des éléments un par un pendant qu'un vérificateur signale ce qui manque encore. Le vérificateur, ce sont quelques règles fixes qui tournent dans cette page et regardent quels éléments contient votre demande ; ce n'est pas une IA. Enfin, vous envoyez la demande complète et comparez les deux réponses. Le jardin et les deux réponses ont été écrits pour ce chapitre afin de montrer le genre de suppositions qu'un modèle peut faire.

## Essayez : Atelier du brief

Marquez ce qu'une demande vague a laissé à deviner, ajoutez des éléments jusqu'à ce que le vérificateur ne trouve plus de vide, puis comparez les deux réponses.

https://agenticademy.org/fr/paths/ai-principles/05-saying-what-you-want

## Vérification

### Question 1 sur 4

Vous demandez à un assistant conversationnel d'IA, qui n'a pas accès à votre agenda ni à vos e-mails : « Écris un rappel pour le déjeuner d'équipe de vendredi. » Sa réponse dit que le déjeuner a lieu à 12:30 dans la grande salle. Vous n'avez jamais donné d'heure ni de lieu. Que s'est-il passé ?

- Il a trouvé le déjeuner dans votre agenda et en a recopié l'heure et le lieu.
- Il sait, grâce à son entraînement, quand et où ont lieu les déjeuners d'équipe dans votre entreprise.
- **✓** Votre demande a laissé des vides, et le modèle les a comblés avec des détails probables.

**Pourquoi:** Le modèle ne savait que ce que vous avez écrit. Là où il restait un vide, il a écrit ce qui est probable en général, ce qui se lit bien et peut être faux. Il ne pouvait rien consulter, et il ne sait rien de votre entreprise si vous ne le lui dites pas.

### Question 2 sur 4

Vous voulez de l'aide pour organiser un dîner d'anniversaire pour votre père. Quelle ligne, ajoutée à votre demande, donne à l'IA le plus d'éléments exploitables ?

- **✓** « Huit personnes, dont une végétarienne, environ 30 euros chacune, près de la gare. Il aime les endroits calmes. »
- « Rends-la vraiment spéciale, le genre de soirée dont il se souviendra pendant des années. »
- « Sois aussi créatif que possible, et surprends-moi avec des idées auxquelles je ne penserais pas. »

**Pourquoi:** La première donne des faits que l'IA ne peut pas deviner : le nombre de personnes, qui mange quoi, le budget, le quartier et ce qu'il aime. « Spéciale » et « créatif » semblent utiles mais ne disent pas ce que vous voulez, donc l'IA doit encore deviner.

### Question 3 sur 4

Quelle ligne décrit la façon de travailler, et non le résultat ou le comportement ?

- « Moins de 100 mots, sous forme de courte liste qui tient sur un écran de téléphone. »
- **✓** « Commence par lister les faits que tu vas utiliser, et demande-moi ceux qui manquent. »
- « Adopte un ton chaleureux et amical, et ne promets rien en mon nom. »

**Pourquoi:** Lister d'abord les faits et poser des questions sur les vides, c'est une façon de mener la tâche. La longueur et la forme relèvent du résultat ; le ton et ce qu'il faut éviter relèvent du comportement. Les trois valent la peine d'être dits ; ils répondent à des questions différentes.

### Question 4 sur 4

Vous collez la lettre de la sortie scolaire de l'an dernier comme exemple et demandez celle de cette année. La nouvelle lettre reprend la date et le prix de l'an dernier. Qu'aurait dû dire votre demande ?

- « Fais mieux que la lettre de l'an dernier, et plus à jour. »
- Rien de plus : un exemple seul montre au modèle quelles parties garder.
- **✓** « Reprends sa mise en page, pas ses faits. La date et le prix de cette année sont ci-dessous. »

**Pourquoi:** Un exemple montre à quoi ressemble un bon résultat, mais pas quelles parties garder. Dites ce qu'il faut en reprendre, ici la mise en page, et donnez les nouveaux faits. « Mieux » et « plus à jour » ne disent pas quoi changer, et un exemple seul peut être recopié en entier, anciens faits compris.

## Ce que vous savez faire maintenant

- Expliquer pourquoi une demande vague obtient une réponse pleine de suppositions.
- Décrire le résultat, la façon de travailler et le comportement que vous attendez d'une IA.
- Ajouter le contexte et un exemple que vous seul pouvez donner, et dire ce qu'il faut reprendre de l'exemple.

---

[Retour à Principes de l'IA](README.md) · [Suivant : Vérifier ce que vous recevez](06-checking-what-you-get.md)
