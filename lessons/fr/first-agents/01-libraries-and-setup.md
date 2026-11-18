# Les bibliothèques, et ce qui tourne ici

Construisez vos premiers agents · Chapitre 1 sur 8 · 15 minutes

Les paquets LangChain.js et LangGraph.js dont un agent a besoin, le rôle de chacun, et leur fonctionnement dans un navigateur.

## Objectifs

- Ouvrez les six paquets de la carte et lisez ce que chacun apporte.
- Exécutez le hello-world dans cette page et lisez la réponse.
- Réussissez la vérification.

## La lecture

### Les paquets

LangChain.js et LangGraph.js sont des bibliothèques JavaScript open source pour construire avec des modèles de langage. Elles se présentent en paquets séparés, vous n'installez donc que ce que vous utilisez.

@langchain/core contient les briques communes : les messages, l'interface des modèles de chat, les outils et les callbacks. @langchain/langgraph est le runtime des agents sous forme de graphes : état, nœuds, arêtes, checkpoints et pauses pour approbation. Un paquet de fournisseur comme @langchain/anthropic ou @langchain/openai vous donne la classe de modèle de chat pour l'API d'un fournisseur ; la même classe ChatOpenAI parle aussi aux services compatibles OpenAI comme OpenRouter, Hugging Face et NVIDIA. zod décrit et valide l'entrée de vos outils ; LangChain transforme un schéma zod en JSON Schema que le modèle lit. langsmith est facultatif et enregistre les traces de chaque exécution.

### Pourquoi une bibliothèque de graphes

Vous pouvez écrire une boucle d'agent à la main : appeler le modèle, exécuter les outils qu'il demande, recommencer. Le chapitre 6 construit cette boucle sous forme de graphe à deux nœuds. LangGraph ajoute ce qui manque à une boucle écrite à la main : un état enregistré après chaque étape, pour qu'une exécution puisse attendre une personne et reprendre plus tard ; des branches qui s'exécutent en parallèle ; et une structure que l'on peut inspecter et dessiner.

Le paquet langchain s'appuie sur LangGraph et propose createAgent, un agent prêt à l'emploi avec middleware. Ce parcours construit d'abord le graphe à la main, pour que vous sachiez ensuite ce que ces aides font pour vous.

### Installer

Dans votre propre projet : npm install @langchain/core @langchain/langgraph @langchain/anthropic zod. Ce parcours utilise @langchain/core 1.2.18, @langchain/langgraph 1.5.1 et zod 4.6.7. Fixez des versions exactes dans les vrais projets : ces bibliothèques évoluent vite.

### Ce qui change dans un navigateur

Tout ce parcours tourne dans votre navigateur, il importe donc LangGraph depuis @langchain/langgraph/web. Le point d'entrée principal transmet la configuration aux appels imbriqués avec AsyncLocalStorage, que les navigateurs n'ont pas. Avec /web, vous passez config explicitement aux appels imbriqués et vous mettez les exécutions en pause avec des points d'arrêt (interruptBefore) au lieu d'interrupt(). Le reste de l'API est identique.

Le modèle est scripté par défaut : vous n'avez besoin d'aucune clé et chaque exécution donne le même résultat. Dans ce chapitre, c'est FakeListChatModel, fourni par LangChain, qui renvoie les réponses que vous lui donnez. À partir du chapitre 2, vous pouvez connecter un vrai modèle avec votre propre clé.

## Essayez : Les paquets

Ouvrez chaque paquet de la carte pour voir ce qu'il apporte, la version utilisée ici et comment l'importer. Puis exécutez le hello-world.

https://agenticademy.org/fr/paths/first-agents/01-libraries-and-setup

## Vérification

### Question 1 sur 4

Quel paquet définit HumanMessage, AIMessage et tool() ?

- @langchain/langgraph
- **✓** @langchain/core
- @langchain/anthropic
- zod

**Pourquoi:** @langchain/core contient les briques communes. LangGraph et les paquets de fournisseurs les importent de là.

### Question 2 sur 4

Vous exécutez un graphe LangGraph dans un navigateur. D'où importez-vous StateGraph ?

- @langchain/langgraph
- langchain
- **✓** @langchain/langgraph/web

**Pourquoi:** Le point d'entrée /web ne dépend pas d'AsyncLocalStorage, absent des navigateurs. Vous passez alors config explicitement aux appels imbriqués.

### Question 3 sur 4

Quel est le rôle de zod dans un agent LangChain ?

- **✓** Il décrit et valide les entrées des outils ; LangChain envoie le schéma au modèle en JSON Schema.
- Il appelle le modèle.
- Il enregistre des checkpoints entre les étapes.

**Pourquoi:** Le schéma sert deux fois : le modèle le lit pour savoir quels arguments envoyer, et votre code s'en sert pour rejeter les mauvais arguments.

### Question 4 sur 4

Pourquoi le hello-world utilise-t-il FakeListChatModel ?

- Il raisonne mieux que les vrais modèles.
- Les vrais modèles ne peuvent pas être appelés depuis un navigateur.
- **✓** Il renvoie des réponses fixes : le code tourne sans clé ni réseau, de la même façon à chaque fois.

**Pourquoi:** Un faux modèle rend le code testable et gratuit à exécuter. Les vrais modèles peuvent être appelés depuis un navigateur avec une clé, comme le montre le chapitre 2.

## Ce que vous savez faire maintenant

- Choisir les paquets d'un agent LangGraph.js et dire ce que fait chacun.
- Les installer avec des versions fixées.
- Expliquer ce qui change quand LangGraph tourne dans un navigateur.
- Exécuter du code LangChain avec un faux modèle, sans clé.

---

[Retour à Construisez vos premiers agents](README.md) · [Suivant : Un appel au modèle](02-one-model-call.md)
