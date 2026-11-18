# Un appel au modèle

Construisez vos premiers agents · Chapitre 2 sur 8 · 15 minutes

Appeler un modèle de chat avec LangChain.js, lire l'AIMessage qu'il renvoie et utiliser votre propre clé en sécurité.

## Objectifs

- Lancez l'appel avec le modèle scripté.
- Trouvez le nombre de tokens dans la réponse.
- Prédisez ce que le code affiche.

## La lecture

### Une interface, de nombreux fournisseurs

Chaque paquet de fournisseur a une classe de modèle de chat, comme ChatAnthropic ou ChatOpenAI. Elles partagent une interface : invoke(messages) envoie une liste de messages et renvoie un AIMessage. Vous pouvez aussi passer une simple chaîne, que LangChain enveloppe dans un HumanMessage. stream(messages) renvoie la même réponse par morceaux, au fil de leur arrivée, pour afficher le texte pendant que le modèle écrit encore.

Les options vont au constructeur : model (le nom du modèle chez ce fournisseur), temperature (0 donne la formulation la plus prévisible ; des valeurs plus hautes varient davantage) et maxTokens (la réponse la plus longue que vous paierez). ChatOpenAI accepte aussi configuration.baseURL, pour que la même classe parle à n'importe quel service compatible OpenAI : OpenRouter, le routeur de Hugging Face ou le catalogue d'API de NVIDIA.

### Ce qui revient

L'AIMessage contient content (le texte, pour une réponse simple), tool\_calls (vide jusqu'au chapitre 5), usage\_metadata et response\_metadata. usage\_metadata compte les tokens, les morceaux de texte qu'un modèle lit et écrit : input\_tokens pour ce que vous avez envoyé, output\_tokens pour la réponse. Les fournisseurs facturent au token, et l'entrée grandit à chaque message envoyé : c'est donc le chiffre à surveiller. response\_metadata contient ce que le fournisseur ajoute, comme le nom du modèle et la raison de son arrêt.

### Les clés

Une clé API est le mot de passe d'un compte payant. Gardez-la hors du code et hors des dépôts : lisez-la depuis une variable d'environnement sur votre machine ou votre serveur. Dans cette page, une clé que vous connectez reste dans cet onglet et va de votre navigateur directement au fournisseur. C'est acceptable pour apprendre, mais une application publique doit appeler les modèles depuis un serveur, pour que sa clé ne soit jamais envoyée aux visiteurs. Des offres gratuites existent, avec des limites de débit.

## Essayez : Une requête et sa réponse

Lancez l'appel et ouvrez l'AIMessage qui revient. Connectez votre propre modèle si vous voulez voir une vraie réponse.

https://agenticademy.org/fr/paths/first-agents/02-one-model-call

## Vérification

Le modèle est un modèle de chat qui répond « Hello. » et indique sa consommation. Qu'affiche ce code ?

Code:

```
const reply = await model.invoke("Say hello.");
console.log(typeof reply.content, reply.usage_metadata.output_tokens > 0);
```

- **✓** string true
- object true
- string undefined
- Il lève une erreur : invoke attend un tableau de messages.

**Pourquoi:** invoke accepte aussi une simple chaîne, que LangChain enveloppe dans un HumanMessage. Pour une réponse texte, content est une chaîne, et usage\_metadata compte les tokens de sortie.

## Ce que vous savez faire maintenant

- Appeler un modèle de chat avec invoke() et stream().
- Lire le contenu et la consommation de tokens d'un AIMessage.
- Diriger ChatOpenAI vers un service compatible OpenAI avec baseURL.
- Expliquer où une clé API doit et ne doit pas se trouver.

---

[Retour à Construisez vos premiers agents](README.md) · [Suivant : Messages et état](03-messages-and-state.md)
