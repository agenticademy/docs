# D'où viennent les modèles

Comprendre les modèles · Chapitre 4 sur 8 · 15 minutes

Un modèle se fabrique par étapes, des données et du pré-entraînement jusqu'au fine-tuning, aux retours, à la distillation et à la quantification, et pour les modèles ouverts, de nombreuses étapes laissent un fichier que vous pouvez examiner.

## Objectifs

- Suivre Olmo 3, un modèle entièrement ouvert, de ses données jusqu'à sa publication.
- Suivre DeepSeek-R1 jusqu'à un petit modèle distillé à partir de lui.
- Suivre Gemma 4 de son modèle de base jusqu'à un fichier quantifié.
- Réussir la vérification.

## La lecture

### Données, tokens et pré-entraînement

Tout modèle commence par des données : de grandes collections de textes, de code et souvent d'images, filtrées et sans doublons. Un tokenizer, comme au chapitre 2, les découpe en tokens.

Le pré-entraînement montre au modèle des milliers de milliards de tokens et ajuste ses poids, aussi appelés paramètres : les nombres qu'il apprend. Il prédit de mieux en mieux le token suivant, la tâche du chapitre 3. Les laboratoires enregistrent des checkpoints, des copies des poids en cours de route. Certains, comme Olmo 3, terminent par un entraînement intermédiaire, une phase plus courte sur un mélange plus petit et choisi, puis par une phase sur des textes longs. Le résultat est un modèle de base, qui continue un texte mais ne suit pas les demandes.

### Post-entraînement

Le post-entraînement apprend à un modèle de base à suivre les demandes. Le fine-tuning supervisé (SFT) l'entraîne sur des exemples de demandes accompagnées de bonnes réponses.

L'apprentissage par renforcement (RL) l'entraîne ensuite avec des récompenses : les réponses qui obtiennent un bon score deviennent plus probables. Dans le RLHF, le RL à partir de retours humains, des personnes comparent des réponses et les scores viennent de leurs choix. Dans le RLAIF, c'est un autre modèle qui donne les retours, parfois guidé par une liste écrite de principes appelée constitution. Le DPO, l'optimisation directe des préférences, apprend à partir de paires d'une meilleure et d'une moins bonne réponse, en une seule étape plus simple.

Les modèles de raisonnement ajoutent du RL avec des récompenses vérifiables (RLVR) : une réponse n'est récompensée que si un programme la confirme, comme un résultat de mathématiques ou du code qui passe ses tests.

### Plus petits et plus légers

La distillation entraîne un petit modèle, l'élève, sur des réponses écrites par un grand, l'enseignant. La quantification stocke chaque poids sur moins de bits, les 0 et les 1 de la mémoire d'un ordinateur, par exemple 4 au lieu de 16, si bien que le fichier rétrécit et tient sur des machines plus petites, avec une légère perte de qualité. L'entraînement tenant compte de la quantification (QAT) apprend à un modèle à moins perdre quand il est quantifié. GGUF est un format de fichier lu par les applications qui font tourner des modèles sur votre propre ordinateur.

### Testés et décrits

Avant la publication, les laboratoires évaluent un modèle sur des benchmarks, des ensembles fixes de questions de test, et font du red teaming : des testeurs essaient de lui faire causer du tort. Une fiche de modèle décrit le modèle, son entraînement et ses résultats ; une fiche système couvre aussi les protections mises en place autour de lui.

Les modèles ouverts publient leurs poids ; les modèles entièrement ouverts publient aussi leurs données et leur code. Les modèles fermés comme Claude, GPT, Gemini et Grok sont aussi entraînés, mais leurs créateurs publient des rapports, pas des poids, donc on ne peut pas suivre leurs arbres fichier par fichier. Le visuel suit trois modèles ouverts.

## Essayez : L'arbre généalogique d'un modèle

Suivez trois arbres réels depuis le haut, une étape à la fois. Chaque étape montre ce qu'elle a utilisé et produit, et où c'est publié.

https://agenticademy.org/fr/paths/understanding-models/04-where-models-come-from

## Vérification

Remettez dans l'ordre les étapes de la fabrication d'un modèle, de la première à la dernière.

1. Rassembler et nettoyer les données, puis les découper en tokens.
2. Pré-entraîner sur la prédiction du token suivant : le résultat est un modèle de base.
3. Faire un fine-tuning sur des exemples de demandes accompagnées de bonnes réponses.
4. L'entraîner encore avec des récompenses : à partir de retours, et de réponses qu'un programme peut vérifier.
5. Distiller un modèle élève plus petit à partir de ses réponses.
6. Quantifier les poids de l'élève sur moins de bits.
7. Évaluer le résultat et faire du red teaming, puis publier sa fiche de modèle.

**Pourquoi:** Le pré-entraînement lit des tokens, donc les données et les tokens viennent d'abord, et il donne un modèle de base. Le fine-tuning sur des exemples apprend à ce modèle à suivre les demandes, et les récompenses le façonnent davantage, comme le SFT, le DPO et le RLVR d'Olmo 3. Seul un modèle terminé peut enseigner à un élève, et la quantification réduit ensuite le fichier de l'élève. Les derniers tests et le red teaming ont lieu avant la publication, et la fiche de modèle en rend compte.

## Ce que vous savez faire maintenant

- Nommer dans l'ordre les étapes qui fabriquent un modèle, des données à la fiche de modèle.
- Expliquer ce que changent le fine-tuning, l'entraînement par retours, la distillation et la quantification.
- Suivre l'arbre généalogique d'un modèle ouvert à partir de sa fiche de modèle, étape par étape.
- Dire pourquoi un modèle entièrement ouvert peut être vérifié à chaque étape et un modèle fermé non.

---

[Retour à Comprendre les modèles](README.md) · [Suivant : Taille et mémoire](05-size-and-memory.md)
