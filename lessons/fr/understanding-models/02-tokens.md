# Les tokens

Comprendre les modèles · Chapitre 2 sur 8 · 14 minutes

Les modèles lisent et écrivent du texte sous forme de tokens, chaque famille de modèles découpe le texte à sa façon, et les prix comme les limites de contexte se comptent en tokens.

## Objectifs

- Découper une phrase avec les tokenizers de six familles de modèles.
- Passer à l'espagnol, au japonais et au code, et comparer les décomptes.
- Réussir la vérification.

## La lecture

### Le texte devient des tokens

Au chapitre précédent, vous avez vu de nombreux types de modèles. Ceux qui travaillent sur du texte commencent tous de la même façon. Un tokenizer, un petit programme fourni avec chaque modèle, découpe le texte en tokens : des morceaux qui sont souvent un mot entier avec l'espace qui le précède, et parfois une partie de mot, un signe ou un chiffre. Chaque token est ensuite remplacé par un nombre, son ID de token. Le modèle ne lit que ces nombres. Quand il écrit, il rend des ID de token, et le tokenizer les transforme en texte.

### Chaque famille a son propre vocabulaire

La liste des morceaux que connaît un tokenizer est son vocabulaire. Elle est construite à partir d'un grand échantillon de texte avant l'entraînement du modèle : les morceaux qui y apparaissent souvent deviennent chacun un token. Chaque famille de modèles, comme GPT, Llama ou Qwen, a son propre tokenizer, et une nouvelle génération peut en recevoir un nouveau. Les six du lab connaissent entre 32 768 et 262 144 morceaux.

Un mot absent du vocabulaire est construit à partir de morceaux plus petits. Tout au bout, il y a les octets, les petits codes qu'un ordinateur utilise pour stocker chaque caractère. Un caractère japonais ou un emoji occupe trois ou quatre octets, si bien qu'un tokenizer qui ne l'a pas a besoin de plusieurs tokens pour un seul caractère.

### Pourquoi le décompte est important

Les fournisseurs fixent le prix de leurs modèles par million de tokens : les tokens que vous envoyez, appelés entrée, et les tokens que le modèle écrit, appelés sortie. La fenêtre de contexte d'un modèle, le maximum de texte qu'il peut recevoir d'un coup, se compte aussi en tokens. Un texte qui demande plus de tokens coûte plus cher et remplit la fenêtre plus vite. Dans le lab, la même demande prend plus de tokens en espagnol et en japonais qu'en anglais dans toutes les familles, et le code et les emoji font tenir moins de caractères dans chaque token que l'anglais.

### Ce que contient le lab

Six tokenizers : celui d'OpenAI pour GPT-4o, un modèle fermé, et ceux de cinq familles à poids ouverts, dont les fichiers de modèle sont publiés : Gemma 4 de Google, Llama 3 de Meta, Qwen3, DeepSeek-V3 et Mistral v3. Leurs découpages ont été enregistrés avec leurs vrais fichiers de tokenizer. La dernière étape est facultative : elle télécharge les fichiers que vous choisissez, en affichant d'abord leur taille, et découpe votre propre texte dans cette page.

## Essayez : Lab des tokenizers

Découpez une phrase avec six tokenizers, puis changez de texte et regardez les décomptes changer.

https://agenticademy.org/fr/paths/understanding-models/02-tokens

## Vérification

Ce mot allemand veut dire « message de confirmation de réservation de table ». GPT-4o fait 10 tokens de la phrase anglaise du lab, environ un par mot. Combien en fait-il de ce seul mot ?

Le mot:

```
Tischreservierungsbestätigungsnachricht

1 mot, 39 lettres
Tokens de GPT-4o : ?
```

- 1, parce que c'est un seul mot.
- **✓** 9, parce qu'il est construit à partir de morceaux plus longs qui sont dans le vocabulaire.
- 39, un par lettre.
- Aucun, parce qu'un modèle laisse de côté les mots qu'il n'a jamais vus.

**Pourquoi:** GPT-4o n'a pas de token unique pour ce mot rare, donc son tokenizer le construit à partir de 9 morceaux qu'il connaît : T, isch, reserv, ierungs, best, ät, igungs, nach, richt. Ce n'est pas 1, parce que seuls les mots courants ont un token à eux. Ce n'est pas 39, parce que les suites de lettres fréquentes sont stockées comme un seul morceau. Et rien n'est laissé de côté : tout texte peut être construit à partir de morceaux, jusqu'aux octets isolés. D'autres familles le découpent autrement : Mistral v3 a besoin de 13 tokens.

## Ce que vous savez faire maintenant

- Expliquer ce que sont un tokenizer, un token et un vocabulaire.
- Dire pourquoi le même texte peut faire un nombre différent de tokens dans deux familles de modèles.
- Expliquer pourquoi les prix et les fenêtres de contexte dépendent du nombre de tokens, et pourquoi les autres langues, le code et les emoji coûtent souvent plus cher.

---

[Retour à Comprendre les modèles](README.md) · [Suivant : Choisir le token suivant](03-next-token.md)
