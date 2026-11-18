# Garde-fous

Construisez vos premiers agents · Chapitre 8 sur 8 · 22 minutes

Des contrôles dans le code autour du modèle : contrôles d'entrée et de sortie, listes d'autorisation d'outils et limites d'étapes, en couches.

## Objectifs

- Activez chaque protection et voyez ce qu'elle arrête.
- Arrêtez les quatre attaques en un seul passage.
- Prédisez ce que fait la limite d'étapes.

## La lecture

### Des règles que le modèle ne peut pas contourner en discutant

Un garde-fou est un contrôle dans votre code, en dehors du modèle. Un prompt peut demander au modèle de suivre une règle, et un message habile peut l'en dissuader. Du code qui refuse un appel ne se laisse dissuader de rien.

### Quatre types de contrôle

Les contrôles d'entrée testent le message de l'utilisateur avant que le modèle le lise, et le refusent ou le signalent. Les contrôles de sortie testent la réponse avant que quiconque la voie : par exemple, chaque montant qu'elle cite doit figurer dans un résultat d'outil, pour que le modèle ne puisse pas en inventer.

Les listes d'autorisation limitent ce qui peut se produire : quels outils peuvent s'exécuter, quels hôtes ils peuvent joindre, quels montants ils peuvent déplacer. Elles refusent par défaut : un outil ou un argument que personne n'a listé est refusé, même quand une instruction cachée dans un ticket le demande. Placez la liste d'autorisation autour du nœud d'outils, pour qu'elle refuse avant que l'outil s'exécute.

Les limites d'étapes arrêtent les boucles qui s'emballent. recursionLimit, dans la config de l'exécution, plafonne les supersteps, et LangGraph lève GraphRecursionError quand la limite est atteinte. Interceptez-la par err.name et transformez-la en arrêt clair. Un budget de tokens ou d'appels au modèle fonctionne de la même façon.

### En couches

Aucun contrôle seul n'attrape tout. Le contrôle d'entrée ne voit jamais le texte qui arrive dans un résultat d'outil ; la liste d'autorisation ne lit pas les réponses. Vous les empilez donc, et quand une couche rate, la suivante peut encore arrêter l'attaque.

Les modèles de sécurité dédiés sont une autre couche : des classifieurs comme les modèles de sécurité de contenu Nemotron de NVIDIA et Llama Nemotron safety guard lisent un message et le classent sûr ou dangereux. Ils attrapent ce qu'un motif rate, mais ce sont aussi des modèles : gardez donc aussi les contrôles dans le code.

## Essayez : Quatre protections, quatre attaques

Lancez quatre attaques contre un agent sans protection. Puis activez une protection par attaque et voyez ce qu'elle arrête. Terminez par un passage qui arrête les quatre.

https://agenticademy.org/fr/paths/first-agents/08-guardrails

## Vérification

Le modèle scripté demande toujours un nouvel appel d'outil. Que se passe-t-il ?

Code:

```
await agent.invoke(
  { messages: [new HumanMessage("Check ticket T-48213")] },
  { recursionLimit: 6 },
);
```

- Il tourne sans fin.
- **✓** Il lève GraphRecursionError après 6 étapes.
- Il renvoie le dernier message après 6 étapes, sans erreur.
- recursionLimit ne s'applique qu'aux sous-graphes.

**Pourquoi:** recursionLimit plafonne le nombre de supersteps. Quand la limite est atteinte, LangGraph lève GraphRecursionError, que votre code intercepte et transforme en arrêt clair.

## Ce que vous savez faire maintenant

- Écrire des contrôles d'entrée et de sortie autour d'un modèle.
- Entourer un nœud d'outils d'une liste d'autorisation qui refuse par défaut.
- Limiter les étapes d'un agent et traiter GraphRecursionError.
- Expliquer pourquoi les garde-fous s'empilent en couches.

---

[Retour à Construisez vos premiers agents](README.md)
