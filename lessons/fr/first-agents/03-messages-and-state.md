# Messages et état

Construisez vos premiers agents · Chapitre 3 sur 8 · 18 minutes

Les quatre types de messages, pourquoi toute la conversation est envoyée à chaque fois, et comment LangGraph fusionne l'état avec des reducers.

## Objectifs

- Changez le message système et comparez les réponses.
- Ajoutez un message et remplacez-en un autre par son id.
- Prédisez ce que renvoie le reducer.

## La lecture

### Quatre types de messages

Une conversation est une liste de messages, et chaque message a un type. SystemMessage contient vos instructions au modèle. HumanMessage est ce que l'utilisateur a écrit. AIMessage est la réponse du modèle ; il peut porter des tool\_calls, des demandes d'exécution d'outils. ToolMessage rapporte le résultat d'un outil, relié à sa demande par tool\_call\_id. Les quatre viennent de @langchain/core/messages.

### Le modèle ne se souvient de rien

Un modèle de chat ne garde aucune mémoire entre deux appels. Chaque appel envoie toute la liste, et la réponse ne dépend que de cette liste. Pour poursuivre une conversation, vous ajoutez la réponse et la question suivante, puis vous renvoyez le tout. C'est aussi pourquoi le nombre de tokens d'entrée augmente à chaque tour.

### État et reducers

Dans LangGraph, les données qu'un graphe transporte d'une étape à l'autre forment son état. Un nœud n'écrase pas l'état : il renvoie une mise à jour, et un reducer décide comment la fusionner avec ce qui est déjà là. Sans reducer, la nouvelle valeur remplace l'ancienne.

Pour les messages, LangGraph fournit MessagesAnnotation, un état à une seule clé, messages, fusionnée par messagesStateReducer. Chaque message reçoit un id ; le reducer en attribue un s'il n'en a pas. Un message avec un nouvel id est ajouté à la fin. Un message dont l'id est déjà dans la liste remplace ce message sur place. Un RemoveMessage avec un id supprime ce message. C'est ainsi qu'un graphe modifie une conversation, par exemple pour retirer d'anciens tours, sans reconstruire la liste.

## Essayez : La pile de messages

Appelez le modèle, changez le message système et appelez-le à nouveau, puis envoyez des mises à jour d'état et regardez le reducer ajouter, remplacer et supprimer des messages.

https://agenticademy.org/fr/paths/first-agents/03-messages-and-state

## Vérification

Que renvoie messagesStateReducer ?

Code:

```
messagesStateReducer(
  [new HumanMessage({ id: "a", content: "Hi" }), new AIMessage({ id: "b", content: "Hello" })],
  [new AIMessage({ id: "b", content: "Hello!" }), new HumanMessage({ id: "c", content: "Thanks" })],
);
```

- Quatre messages : a, b, b, c.
- **✓** Trois messages : a, b (désormais « Hello! »), c.
- Deux messages : b et c.
- Il lève une erreur, car b apparaît deux fois.

**Pourquoi:** La mise à jour avec l'id b remplace le b existant sur place, et c est nouveau, donc il est ajouté.

## Ce que vous savez faire maintenant

- Utiliser correctement les quatre types de messages.
- Expliquer pourquoi la conversation est envoyée à chaque appel.
- Prédire comment le reducer de messages de LangGraph fusionne une mise à jour.

---

[Retour à Construisez vos premiers agents](README.md) · [Suivant : Votre premier graphe](04-your-first-graph.md)
