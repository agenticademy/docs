# Des boucles qui tiennent

Agents avancés · Chapitre 5 sur 10 · 20 minutes

Les politiques de nouvelle tentative, les timeouts, les gestionnaires d'erreurs et les checkpoints maintiennent une longue exécution quand un outil échoue ou que le processus s'arrête.

## Objectifs

- Injectez un outil instable et observez les nouvelles tentatives.
- Envoyez l'échec vers une étape de compensation.
- Arrêtez l'exécution en cours de route et reprenez-la depuis son checkpoint.
- Prédisez les tentatives et le chemin.

## La lecture

### Nouvelles tentatives et timeouts

Un appel d'outil peut échouer un instant : un 503 d'un service surchargé, ou aucune réponse. Dans LangGraph.js 1.5.1, un nœud peut porter une retryPolicy : maxAttempts (la première tentative compte), initialInterval et backoffFactor pour les attentes entre les tentatives, et retryOn pour choisir les erreurs qui valent un nouvel essai. Un timeout transforme un appel bloqué en tentative échouée. runTimeout limite la durée totale d'une tentative ; idleTimeout limite la durée sans progrès, comme une écriture d'état, un événement de callback ou un appel à heartbeat(). Les deux lèvent NodeTimeoutError, que la politique de nouvelle tentative traite comme toute autre erreur.

### Gestionnaires d'erreurs et compensation

Quand les tentatives sont épuisées, l'errorHandler du nœud, ajouté dans LangGraph.js 1.4, s'exécute au lieu de faire échouer l'exécution. Il reçoit l'état d'entrée du nœud et un NodeError, qui contient le nom du nœud et l'erreur, et renvoie une mise à jour d'état ou un Command({ goto }). Envoyer l'exécution vers une compensation, c'est le pattern saga : une étape qui défait le travail fait avant l'échec, comme libérer une chambre réservée mais jamais payée.

### Arrêter et reprendre

Avec un checkpointer, chaque superstep se termine en enregistrant un checkpoint sur le thread. Passez un RunControl dans la config de l'exécution et appelez requestDrain() : les nœuds déjà en cours terminent, le checkpoint est enregistré et l'exécution lève GraphDrained. Un nouvel appel avec une entrée null sur le même thread\_id commence au nœud suivant ; les nœuds terminés ne s'exécutent pas de nouveau. Avec un checkpointer qui enregistre dans une base de données plutôt qu'en mémoire, cela fonctionne aussi depuis un nouveau processus.

Un crash ne prévient pas ainsi. Le nœud en cours perd ses écritures et s'exécute de nouveau à la reprise, donc un appel avec des effets de bord devrait porter une clé d'idempotence : une clé dont le service se souvient, pour qu'une requête répétée reçoive le premier résultat au lieu d'agir deux fois.

### Dans ce lab

Un graphe de réservation : le modèle planifie le voyage, puis reserve, charge et confirm appellent chacun un service. Les navigateurs n'ont pas AsyncLocalStorage, donc interrupt() et task() de l'API fonctionnelle ne peuvent pas tourner ici. Le lab utilise l'API de graphe et passe la config à chaque appel à la main. Son deuxième processus est un nouveau graphe sur le même MemorySaver en mémoire, et débrancher annule l'AbortSignal de l'exécution.

## Essayez : Échouer, se rétablir, reprendre

Rendez un outil instable et observez les nouvelles tentatives, coupez le service de paiement et suivez le chemin jusqu'à la compensation, puis arrêtez le processus et reprenez le thread.

https://agenticademy.org/fr/paths/advanced-agents/05-loops-that-survive

## Vérification

charge\_card lève un 503 à chaque appel. Que se passe-t-il quand ce code s'exécute ?

Code:

```
const graph = new StateGraph(Trip)
  .addNode("reserve", reserve)
  .addNode("charge", charge, {
    retryPolicy: {
      maxAttempts: 2,
      initialInterval: 100,
    },
    errorHandler: () =>
      new Command({ goto: "release" }),
  })
  .addNode("release", release)
  .addEdge(START, "reserve")
  .addEdge("reserve", "charge")
  .addEdge("charge", END)
  .addEdge("release", END)
  .compile();

await graph.invoke(input);
```

- **✓** charge\_card est appelé deux fois ; puis l'errorHandler envoie l'exécution vers release, et invoke se termine normalement.
- charge\_card est appelé trois fois, le premier essai et deux nouvelles tentatives ; puis release s'exécute.
- charge\_card est appelé deux fois ; puis invoke lève le 503.
- L'errorHandler s'exécute après le premier 503, puis charge est tenté de nouveau.

**Pourquoi:** maxAttempts compte toutes les tentatives, la première comprise, donc charge\_card est appelé deux fois, pas trois. L'errorHandler ne s'exécute qu'une fois ces tentatives épuisées, jamais entre elles. Son Command envoie l'exécution vers release, qui se termine à END, donc invoke renvoie l'état. Un nœud sans errorHandler fait lever le 503 à invoke une fois ses tentatives épuisées.

## Ce que vous savez faire maintenant

- Donner à un nœud une retryPolicy et un timeout, et prédire combien de tentatives il fait.
- Envoyer le dernier échec d'un nœud vers une étape de compensation avec errorHandler et Command.
- Arrêter une exécution avec RunControl.requestDrain et la reprendre depuis son checkpoint sans répéter les nœuds terminés.
- Utiliser des clés d'idempotence pour qu'un nœud qui s'exécute de nouveau après un crash n'agisse pas deux fois.

---

[Retour à Agents avancés](README.md) · [Suivant : Des boucles qui s'arrêtent](06-loops-that-stop.md)
