# Choisir le token suivant

Comprendre les modèles · Chapitre 3 sur 8 · 14 minutes

Un modèle donne un score à chaque token suivant possible, transforme les scores en probabilités et en tire un au sort ; la température, le top-k et le top-p changent ce tirage, et les modèles de base, instruct et de raisonnement sont entraînés à écrire différemment.

## Objectifs

- Voir les 20 tokens suivants les plus probables pour un prompt.
- Déplacer la température et le top-p, puis tirer à nouveau.
- Poser la même question à un modèle de base et à un modèle instruct.
- Réussir la vérification.

## La lecture

### Un token à la fois

Dans le chapitre précédent, le texte est devenu des tokens. Un modèle écrit de la même façon : il lit tout le texte jusqu'ici, choisit un token suivant, l'ajoute au texte et recommence. Une réponse de 100 tokens demande 100 de ces étapes.

### Des scores aux probabilités

À chaque étape, le modèle donne à chaque token qu'il connaît un score, appelé logit. SmolLM2-360M, le petit modèle ouvert de ce chapitre, connaît 49 152 tokens, il donne donc 49 152 scores. Les scores sont ensuite transformés en chances, aussi appelées probabilités, dont la somme fait 100 %. Un score plus élevé veut dire une probabilité plus grande : un point de plus donne environ 2,7 fois la probabilité.

### Un tirage au sort, avec des réglages

Puis un token est tiré au sort, selon ces probabilités, comme un ticket tiré d'un sac où les tokens probables ont plus de tickets. Le même prompt peut donc recevoir des réponses différentes. Trois réglages changent le tirage. La température divise chaque score avant qu'il ne devienne une probabilité : en dessous de 1, les tokens probables deviennent encore plus probables ; au-dessus de 1, les probabilités s'égalisent. Le top-k ne garde que les k tokens les plus probables. Le top-p garde les tokens les plus probables jusqu'à ce que leurs probabilités atteignent p au total, donc un top-p de 0,9 écarte la longue traîne des tokens improbables. Ce qui reste est remis à l'échelle pour faire 100 %.

### Modèles de base et instruct

Un modèle de base a seulement appris à continuer un texte, donc une question peut recevoir d'autres questions. Un modèle instruct a été entraîné en plus sur des conversations, écrites dans un format de chat qui indique qui parle. Il répond, puis écrit un token de fin, un token spécial qui veut dire qu'il a terminé.

### Modèles de raisonnement

Un modèle de raisonnement écrit d'abord son raisonnement, de la même façon, token par token, puis la réponse. Au 2026-11-03, des modèles fermés comme Claude Opus 5.5 et GPT-6.1 Sol font du raisonnement un réglage, et des modèles ouverts comme DeepSeek-R1 et Olmo 3 7B Think sont publiés comme des modèles de raisonnement distincts.

### Dans le visuel

Les nombres du visuel ont été enregistrés avec SmolLM2-360M et sa version instruct, tous deux de Hugging Face. Si votre navigateur a WebGPU, un moyen pour les pages web d'utiliser la carte graphique, vous pouvez aussi télécharger le modèle instruct et le faire tourner en direct.

## Essayez : Tirage du token suivant

Choisissez un prompt et lisez les probabilités, puis façonnez-les et tirez, un token à la fois. Enfin, posez la même question au modèle de base et au modèle instruct.

https://agenticademy.org/fr/paths/understanding-models/03-next-token

## Vérification

Vous tirez le token suivant 100 fois avec ces réglages. Qu'obtenez-vous ?

Les réglages:

```
Prompt: The capital of France is
Modèle: SmolLM2-360M (base)

Token suivant   Probabilité
Paris           20,3 %
the             19,6 %
located         10,5 %
situated         4,7 %
called           3,7 %
… 15 de plus, puis tout le reste

Température: 1
Top-k:       désactivé
Top-p:       0,5
```

- « Paris » les 100 fois : c'est le token le plus probable.
- « Paris » et « the », environ 50 fois chacun.
- **✓** « Paris », « the » et « located », environ 40, 39 et 21 fois.
- N'importe lequel des 49 152 tokens, chacun aussi souvent que sa probabilité l'indique.

**Pourquoi:** Le top-p garde les tokens les plus probables jusqu'à ce que leurs probabilités atteignent p. « Paris » et « the » totalisent 39,9 %, moins de 50 % ; avec « located », le total est de 50,4 %, donc ces trois-là restent et le reste est écarté. Les trois sont remis à l'échelle pour faire 100 % : environ 40 %, 39 % et 21 %. « Paris » à chaque fois demanderait un réglage qui garde un seul token, comme un top-k à 1 ou un top-p à 0,2, et n'importe quel token ne pourrait sortir qu'avec un top-p à 1.

## Ce que vous savez faire maintenant

- Expliquer comment un modèle transforme des scores en probabilités et tire le token suivant.
- Dire ce que la température, le top-k et le top-p font aux probabilités.
- Distinguer la sortie d'un modèle de base de celle d'un modèle instruct, et dire ce qu'ajoute un modèle de raisonnement.

---

[Retour à Comprendre les modèles](README.md) · [Suivant : D'où viennent les modèles](04-where-models-come-from.md)
