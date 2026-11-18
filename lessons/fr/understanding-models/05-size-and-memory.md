# Taille et mémoire

Comprendre les modèles · Chapitre 5 sur 8 · 15 minutes

La mémoire dont un modèle a besoin dépend de ses paramètres et des bits qui stockent chacun d'eux, plus de la place pour son contexte ; un mélange d'experts utilise quelques-uns de ses paramètres pour chaque token, mais les garde tous en mémoire.

## Objectifs

- Faire descendre Llama-3.1-8B jusqu'à un type 4 bits et voir son fichier rétrécir.
- Vérifier quelles versions tiennent dans 8, 16, 24 et 80 Go.
- Comparer les paramètres totaux et actifs de trois modèles à mélange d'experts.
- Réussir le test.

## La lecture

### Les paramètres prennent de la mémoire

Au chapitre 1, vous avez découvert les paramètres, les nombres qu'un modèle a appris, aussi appelés poids. Un nom en donne souvent le nombre : Llama-3.1-8B en a environ 8 milliards. Pour tourner, un modèle doit garder tous ses poids en mémoire : le stockage rapide avec lequel travaille un ordinateur, ou la mémoire d'une carte graphique (un GPU, une puce qui fait beaucoup de calculs à la fois). La mémoire se compte en octets, et un octet fait 8 bits. Les poids demandent donc paramètres × bits par poids ÷ 8 octets. À 16 bits chacun, 8 milliards de poids prennent 16 Go, soit 16 milliards d'octets.

### Moins de bits, un peu moins bien

La quantification, vue au chapitre 4, stocke chaque poids sur moins de bits. Moins de bits peuvent contenir moins de valeurs, donc chaque poids est arrondi plus grossièrement : le fichier rétrécit, et les réponses deviennent un peu moins bonnes, surtout avec le moins de bits. Le visuel utilise des mesures de llama.cpp, un programme gratuit qui fait tourner des modèles sur votre propre ordinateur. Les noms de ses types donnent les bits : Q4\_K\_M garde la plupart des poids sur 4 bits et certains sur plus, plus un petit supplément pour chaque bloc de poids : 4,89 bits en moyenne. Il donne les tailles en Gio : 1 Gio vaut 1 073 741 824 octets, environ 7 % de plus que 1 Go.

### De la place pour travailler

Un modèle tient quand ses poids, plus un peu de place, tiennent dans la mémoire dont vous disposez. Sur un ordinateur portable, cette mémoire est partagée avec tout ce qui tourne par ailleurs. La place sert au contexte : pendant qu'un modèle travaille, il garde ce qu'il a calculé pour chaque token, et cette réserve grandit à chaque token, jusqu'à la fenêtre de contexte du chapitre 2.

### Beaucoup d'experts, quelques-uns à la fois

Un modèle à mélange d'experts divise la plupart de ses couches en experts, des groupes de poids plus petits. Pour chaque token, une petite partie appelée routeur choisit quelques experts, et seuls ceux-là travaillent. Leurs paramètres, plus les parties que chaque token utilise, sont les paramètres actifs ; tous ensemble forment le total. Un token demande à peu près autant de travail que dans un modèle de la taille active, mais le routeur peut choisir n'importe quel expert pour le token suivant, donc le total doit être en mémoire.

### Fermés et ouverts

Les modèles fermés comme Claude, GPT, Gemini et Grok ne publient pas leurs poids, et leurs pages ne donnent pas leur nombre de paramètres, donc chaque nombre de paramètres du visuel vient de la page d'un modèle ouvert, à la date du 2026-11-03.

## Essayez : Est-ce que ça tient ?

Faites passer un modèle à moins de bits, essayez quatre tailles de mémoire, puis voyez quelle part d'un grand modèle travaille pour chaque token.

https://agenticademy.org/fr/paths/understanding-models/05-size-and-memory

## Vérification

Ce modèle à mélange d'experts est quantifié à environ 4,9 bits par poids. Quelle est la plus petite de ces mémoires qui contient ses poids ?

Le modèle:

```
Modèle :    gpt-oss-120b, poids ouverts
Total :     117 milliards de paramètres
Actifs :    5,1 milliards par token
Chiffres :  fiche du modèle, 2026-11-03
Quantifié : 4,9 bits par poids

Mémoire :   8 Go · 16 Go · 24 Go · 80 Go
```

- 8 Go : seuls les 5,1 milliards de paramètres actifs doivent être en mémoire, environ 3,1 Go.
- 16 Go : les 5,1 milliards de paramètres actifs à 16 bits chacun, environ 10,2 Go.
- **✓** 80 Go : les 117 milliards de paramètres doivent tous être en mémoire, environ 72 Go.
- Aucune : 117 milliards × 4,9 bits font environ 573 Go.

**Pourquoi:** Le routeur peut choisir n'importe quel expert pour le token suivant, donc les 117 milliards de paramètres doivent tous être en mémoire, pas seulement les 5,1 milliards qu'un token utilise : 117 milliards × 4,9 bits ÷ 8 bits par octet ≈ 72 Go, ce qui laisse environ 8 Go sur les 80 pour le contexte. 8 Go et 16 Go ne contiendraient que la partie active, à 4,9 ou à 16 bits. 573 Go oublie de diviser par 8, les bits d'un octet.

## Ce que vous savez faire maintenant

- Estimer la mémoire dont les poids d'un modèle ont besoin à partir de ses paramètres et de ses bits par poids.
- Dire ce que la quantification fait gagner et ce qu'elle coûte.
- Dire si un modèle tient dans une mémoire donnée, avec de la place pour son contexte.
- Expliquer les paramètres totaux et actifs d'un mélange d'experts, et pourquoi le total fixe la mémoire.

---

[Retour à Comprendre les modèles](README.md) · [Suivant : Modèles ouverts et fermés](06-open-and-closed.md)
