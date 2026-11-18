# Comprendre les modèles

Débutant · Sans code · 8 chapitres · 120 min

Pour toute personne qui veut savoir ce qu'il y a dans les modèles derrière les outils d'IA. Vous voyez comment un texte devient des tokens, comment un modèle choisit le suivant, comment les modèles sont fabriqués, quelle est leur taille et ce que permettent leurs licences, puis vous choisissez un modèle pour un agent. Les exemples associent des modèles fermés et des modèles ouverts, et chaque chiffre vient d'un instantané daté.

## Avant de commencer

Rien. Les deux premiers chapitres de Principes de l'IA aident.

## À la fin, vous saurez

- Nommer les types de modèles qu'utilise un agent, et le rôle de chacun.
- Expliquer les tokens, comment un modèle choisit le suivant et ce que change la température.
- Suivre la fabrication d'un modèle, des données et du pré-entraînement jusqu'à un fichier quantifié.
- Distinguer modèles fermés, poids ouverts et open source, et lire les clauses clés d'une licence.
- Lire un classement avec prudence, et choisir un modèle pour un agent selon ses capacités, son coût et ses règles d'usage.

## Chapitres

1. [Les types de modèles](01-kinds-of-models.md): Les modèles de chat, de raisonnement, multimodaux et d'embedding, les rerankers, les modèles vocaux, d'image et de garde, et les petits modèles font des tâches différentes, et un agent en utilise souvent plusieurs.
2. [Les tokens](02-tokens.md): Les modèles lisent et écrivent du texte sous forme de tokens, chaque famille de modèles découpe le texte à sa façon, et les prix comme les limites de contexte se comptent en tokens.
3. [Choisir le token suivant](03-next-token.md): Un modèle donne un score à chaque token suivant possible, transforme les scores en probabilités et en tire un au sort ; la température, le top-k et le top-p changent ce tirage, et les modèles de base, instruct et de raisonnement sont entraînés à écrire différemment.
4. [D'où viennent les modèles](04-where-models-come-from.md): Un modèle se fabrique par étapes, des données et du pré-entraînement jusqu'au fine-tuning, aux retours, à la distillation et à la quantification, et pour les modèles ouverts, de nombreuses étapes laissent un fichier que vous pouvez examiner.
5. [Taille et mémoire](05-size-and-memory.md): La mémoire dont un modèle a besoin dépend de ses paramètres et des bits qui stockent chacun d'eux, plus de la place pour son contexte ; un mélange d'experts utilise quelques-uns de ses paramètres pour chaque token, mais les garde tous en mémoire.
6. [Modèles ouverts et fermés](06-open-and-closed.md): Certains modèles ne s'utilisent que par le service de leur créateur, d'autres partagent leurs poids sous des licences plus ou moins strictes, et quelques-uns sont open source, avec leurs données et leur code.
7. [Comparer des modèles](07-comparing-models.md): Les benchmarks et les classements aident à comparer des modèles, mais les marges d'erreur, les réglages d'effort, le coût, les tests divulgués, les tests saturés, le programme du test et qui l'a lancé changent tous le sens d'un score.
8. [Choisir un modèle pour un agent](08-choosing-for-an-agent.md): Choisir un modèle, c'est écarter ceux qui échouent à une limite stricte, puis peser la capacité, la vitesse, le prix par tâche, le contexte et la fiabilité avec les outils pour le travail d'un agent.

https://agenticademy.org/fr/paths/understanding-models
