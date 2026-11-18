# Modèles ouverts et fermés

Comprendre les modèles · Chapitre 6 sur 8 · 15 minutes

Certains modèles ne s'utilisent que par le service de leur créateur, d'autres partagent leurs poids sous des licences plus ou moins strictes, et quelques-uns sont open source, avec leurs données et leur code.

## Objectifs

- Choisir un objectif, comme fonctionner hors ligne ou lancer une application commerciale.
- Vérifier les modèles niveau par niveau et voir lesquels conviennent à cet objectif.
- Ouvrir une licence et lire ses termes clés.
- Réussir la vérification.

## La lecture

### Fermé : un service que vous appelez

Au chapitre 4, les modèles ouverts publiaient leurs poids, les nombres qu'un modèle a appris, et les modèles fermés non. Un modèle fermé tourne sur les ordinateurs de son créateur. Vous y accédez par l'application du créateur ou par son API, un service auquel votre programme envoie du texte, et vous payez par token selon les conditions d'utilisation du créateur. Claude, GPT, Gemini et Grok fonctionnent ainsi.

### Poids ouverts, sous licence

Un modèle à poids ouverts vous laisse télécharger ses poids. Vous pouvez le faire tourner sur votre propre ordinateur sans connexion internet, et lui appliquer un fine-tuning : l'entraîner davantage sur vos propres exemples. Une licence accompagne les poids, le texte juridique qui dit ce que vous pouvez en faire.

Une licence permissive, comme Apache-2.0 ou MIT, autorise presque tout usage, commercial aussi, tant que vous conservez sa notice. Une licence restrictive ajoute des conditions, comme un accord séparé au-delà d'un nombre d'utilisateurs ou d'un montant de revenus, un nom à afficher, ou des usages interdits. Gemma 4, gpt-oss et DeepSeek-V4.1-Flash ont des licences permissives ; Llama 4 et Kimi K3 ont des licences restrictives.

### Open source

Poids ouverts ne veut pas dire open source. L'Open Source Initiative (OSI), qui tient la définition du logiciel open source, a rédigé l'Open Source AI Definition 1.0. Elle demande trois choses sous des conditions ouvertes : les poids, le code qui entraîne et fait tourner le modèle, et des informations sur les données, assez sur les données d'entraînement pour qu'une personne compétente construise un modèle similaire. Peu de modèles y répondent. Olmo 3, que vous avez suivi au chapitre 4, publie les trois.

### Les obtenir et les faire tourner

Les modèles ouverts sont partagés sur des plateformes comme Hugging Face, un site web qui héberge les fichiers, la fiche de modèle et la licence de chaque modèle. Certains sont à accès restreint : vous acceptez la licence, et parfois attendez l'accord du créateur, avant que le téléchargement commence. Sur votre ordinateur, des applications les font tourner à partir de fichiers faits pour elles : llama.cpp et Ollama lisent des fichiers GGUF, MLX fait tourner des modèles sur les Mac avec Apple silicon, et un navigateur avec WebGPU peut faire tourner les petits, comme au chapitre 3. Savoir si un modèle tient en mémoire est la question du chapitre 5.

### Dans le visuel

Vous choisissez un objectif et vérifiez des modèles réels niveau par niveau, du fermé à l'open source. Puis vous ouvrez une licence. Les faits viennent des pages des modèles et des licences, dans un instantané daté du 2026-11-03. C'est un résumé pour apprendre, pas un conseil juridique.

## Essayez : Matrice des licences

Choisissez un objectif, vérifiez les modèles niveau par niveau du fermé à l'open source, puis ouvrez une licence et lisez ses termes clés.

https://agenticademy.org/fr/paths/understanding-models/06-open-and-closed

## Vérification

### Question 1 sur 3

Il vous faut un modèle sur un ordinateur portable sans connexion internet. Quel type peut le faire ?

- Un modèle fermé, utilisé par son API depuis le portable.
- **✓** Un modèle à poids ouverts ou open source, téléchargé d'abord sur le portable.
- N'importe quel modèle, tant que vous avez une clé d'API pour lui.
- Aucun : chaque modèle a besoin d'internet pour répondre.

**Pourquoi:** Fonctionner hors ligne demande les poids sur le portable, et seuls les modèles à poids ouverts et open source permettent de les télécharger. Un modèle fermé tourne sur les serveurs de son créateur, donc son API demande une connexion, avec ou sans clé. Un modèle ouvert téléchargé n'en demande aucune, si le portable a la mémoire pour lui.

### Question 2 sur 3

Les poids d'un modèle sont publiés sous Apache-2.0, mais rien n'est dit sur ses données d'entraînement. À quel niveau est-il ?

- Fermé, puisque rien n'est dit sur ses données d'entraînement.
- Poids ouverts, licence restrictive.
- Open source, selon la définition de l'OSI.
- **✓** Poids ouverts, licence permissive.

**Pourquoi:** Fermé signifie que les poids restent chez le créateur, et ceux-ci sont publiés, quoi qu'on dise des données. Apache-2.0 autorise l'usage commercial sans limite d'utilisateurs ni de revenus, donc la licence est permissive, pas restrictive. Ce n'est pas de l'open source : la définition de l'OSI demande aussi des informations sur les données et le code d'entraînement, et une licence permissive seule ne les donne pas.

### Question 3 sur 3

La licence de Llama 4 dit qu'au-delà de 700 millions d'utilisateurs actifs mensuels, il faut une licence séparée de Meta. Une start-up de 20 000 utilisateurs crée une application payante sur Llama 4. Que signifie cette condition pour elle ?

- Elle ne peut pas utiliser Llama 4 dans une application payante tant que Meta ne lui donne pas une licence séparée.
- Elle doit payer Meta par token pour utiliser Llama 4, comme avec un modèle fermé.
- **✓** La limite d'utilisateurs ne s'applique pas à elle, mais les autres termes de la licence s'appliquent déjà.
- Rien dans la licence ne s'applique à elle tant qu'elle n'a pas 700 millions d'utilisateurs mensuels.

**Pourquoi:** La limite commence au-delà de 700 millions d'utilisateurs actifs mensuels, donc une petite start-up peut créer une application payante sans licence séparée. Les autres termes s'appliquent dès le premier utilisateur : la licence de Llama 4 demande d'afficher « Built with Llama » et de suivre la politique d'utilisation de Meta. Les poids ouverts ne se paient pas par token : vous les faites tourner vous-même.

## Ce que vous savez faire maintenant

- Distinguer les modèles fermés, les poids ouverts sous licence restrictive ou permissive, et l'open source.
- Dire quels types de modèles conviennent à un objectif : une application commerciale, un usage hors ligne, le fine-tuning ou des données d'entraînement connues.
- Lire les termes clés d'une licence, comme les limites d'utilisateurs et de revenus, avant de construire sur un modèle.
- Nommer où les modèles ouverts sont partagés et les applications qui les font tourner sur votre propre ordinateur.

---

[Retour à Comprendre les modèles](README.md) · [Suivant : Comparer des modèles](07-comparing-models.md)
