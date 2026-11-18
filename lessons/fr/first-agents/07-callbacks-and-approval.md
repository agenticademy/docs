# Callbacks et approbation humaine

Construisez vos premiers agents · Chapitre 7 sur 8 · 22 minutes

Observer une exécution avec des callbacks, et la mettre en pause avant un outil risqué pour qu'une personne l'approuve ou la refuse.

## Objectifs

- Attachez un gestionnaire de callbacks et observez ses événements.
- Mettez en pause avant l'outil de remboursement, puis approuvez-le.
- Refusez un remboursement et voyez comment l'agent le gère.
- Réussissez la vérification.

## La lecture

### Les callbacks observent

Un gestionnaire de callbacks est un objet dont LangChain appelle les méthodes au fil d'une exécution : une sous-classe de BaseCallbackHandler avec des méthodes comme handleChatModelStart, handleLLMEnd, handleToolStart et handleToolEnd. Vous le passez dans config.callbacks, et il atteint chaque appel imbriqué qui reçoit cette config. Servez-vous-en pour les journaux, les métriques et les traces. Un callback ne fait qu'observer : LangChain ignore ce que renvoient ses méthodes et, par défaut, se contente de journaliser une erreur levée dans l'une d'elles ; il ne peut donc ni modifier ni arrêter une étape.

### Les points d'arrêt mettent en pause

Pour arrêter une exécution avant une étape, LangGraph doit enregistrer l'état et y revenir. C'est le rôle d'un checkpointer : compile({ checkpointer: new MemorySaver() }) enregistre un checkpoint après chaque superstep, sous le thread\_id de config.configurable. Ajoutez interruptBefore: ["approval\_tools"] et l'exécution s'arrête avant ce nœud.

Pendant la pause, (await graph.getState(config)).next indique ce qui attend, et le dernier AIMessage contient l'appel en attente. Pour approuver, reprenez avec graph.invoke(null, config) : une entrée null signifie « continuer depuis le checkpoint ». Pour refuser, appelez d'abord graph.updateState(config, { messages: [toolMessage] }, "approval\_tools") : votre ToolMessage répond à l'appel comme si le nœud s'était exécuté, le nœud est donc sauté et le modèle lit votre refus.

Donnez aux outils risqués leur propre nœud, comme ici avec create\_refund, pour que la barrière n'arrête qu'eux.

### Dans Node, et ailleurs

Dans Node, createAgent du paquet langchain offre des hooks de middleware (beforeModel, afterModel, wrapModelCall, wrapToolCall) et humanInTheLoopMiddleware, qui met en pause avec interrupt(). interrupt() a besoin d'AsyncLocalStorage : dans un navigateur, vous utilisez donc des points d'arrêt, comme ici. Les agents de code reprennent la même idée : des hooks qui s'exécutent avant un appel d'outil et peuvent le refuser.

## Essayez : Une barrière avant le remboursement

Quatre étapes, une action chacune, et une cinquième facultative : observez les événements de callback d'une exécution, placez une barrière avant le remboursement, approuvez une exécution, puis refusez la suivante.

https://agenticademy.org/fr/paths/first-agents/07-callbacks-and-approval

## Vérification

### Question 1 sur 3

De quoi interruptBefore a-t-il besoin pour fonctionner ?

- **✓** D'un checkpointer, et d'un thread\_id dans la config.
- D'un gestionnaire de callbacks.
- D'un vrai modèle.

**Pourquoi:** Mettre en pause, c'est enregistrer l'état et y revenir. Le checkpointer le stocke sous l'id du thread.

### Question 2 sur 3

L'exécution est en pause avant approval\_tools. Comment la poursuivre telle quelle ?

- Rappeler invoke avec l'entrée d'origine.
- **✓** graph.invoke(null, config) avec le même thread\_id.
- Démarrer un nouveau thread.

**Pourquoi:** Une entrée null indique à LangGraph de reprendre depuis le checkpoint enregistré au lieu de lancer une nouvelle exécution.

### Question 3 sur 3

Le handleToolStart d'un gestionnaire de callbacks renvoie false quand il voit create\_refund. Que se passe-t-il ?

- Le remboursement est bloqué.
- **✓** Rien ne change : LangChain ignore la valeur renvoyée, donc le remboursement s'exécute. Pour arrêter une étape, mettez le graphe en pause ou protégez l'outil.
- L'exécution se met en pause avant approval\_tools.

**Pourquoi:** Les callbacks servent à observer et à journaliser. Le contrôle appartient au graphe : un point d'arrêt avec updateState, comme ici, ou un contrôle dans le nœud d'outils.

## Ce que vous savez faire maintenant

- Journaliser les événements de modèle et d'outils d'une exécution avec un gestionnaire de callbacks.
- Mettre un graphe en pause avant un nœud avec un checkpointer et interruptBefore.
- Reprendre ou refuser une étape en attente avec getState, updateState et invoke(null).

---

[Retour à Construisez vos premiers agents](README.md) · [Suivant : Garde-fous](08-guardrails.md)
