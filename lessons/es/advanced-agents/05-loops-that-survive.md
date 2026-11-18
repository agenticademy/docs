# Bucles que sobreviven

Agentes avanzados · Capítulo 5 de 10 · 20 minutos

Las políticas de reintento, los timeouts, los manejadores de errores y los checkpoints mantienen en marcha una ejecución larga cuando una herramienta falla o el proceso se detiene.

## Objetivos

- Inyecta una herramienta inestable y mira los reintentos.
- Envía el fallo a un paso de compensación.
- Detén la ejecución a la mitad y reanúdala desde su checkpoint.
- Predice los intentos y la ruta.

## La lectura

### Reintentos y timeouts

Una llamada a una herramienta puede fallar por un momento: un 503 de un servicio ocupado, o ninguna respuesta. En LangGraph.js 1.5.1 un nodo puede llevar una retryPolicy: maxAttempts (el primer intento cuenta), initialInterval y backoffFactor para las esperas entre intentos, y retryOn para elegir los errores que merecen otro intento. Un timeout convierte una llamada colgada en un intento fallido. runTimeout limita el tiempo total de un intento; idleTimeout limita el tiempo sin progreso, como una escritura de estado, un evento de callback o una llamada a heartbeat(). Ambos lanzan NodeTimeoutError, que la política de reintento trata como cualquier otro error.

### Manejadores de errores y compensación

Cuando se agotan los intentos, el errorHandler del nodo, añadido en LangGraph.js 1.4, se ejecuta en lugar de hacer fallar la ejecución. Recibe el estado de entrada del nodo y un NodeError, que contiene el nombre del nodo y el error, y devuelve una actualización de estado o un Command({ goto }). Enviar la ejecución a una compensación es el patrón saga: un paso que deshace el trabajo hecho antes del fallo, como liberar una habitación que se reservó pero nunca se pagó.

### Detener y reanudar

Con un checkpointer, cada superstep termina guardando un checkpoint en el thread. Pasa un RunControl en la config de la ejecución y llama a requestDrain(): los nodos que ya se están ejecutando terminan, se guarda el checkpoint y la ejecución lanza GraphDrained. Si vuelves a invocar con entrada null en el mismo thread\_id, empieza en el nodo siguiente; los nodos que terminaron no se vuelven a ejecutar. Con un checkpointer que guarda en una base de datos en lugar de en memoria, esto funciona también desde un proceso nuevo.

Una caída no avisa así. El nodo que se estaba ejecutando pierde sus escrituras y se vuelve a ejecutar al reanudar, así que una llamada con efectos secundarios debería llevar una clave de idempotencia: una clave que el servicio recuerda, para que una solicitud repetida reciba el primer resultado en lugar de actuar dos veces.

### En este laboratorio

Un grafo de reservas: el modelo planifica el viaje, y luego reserve, charge y confirm llaman cada uno a un servicio. Los navegadores no tienen AsyncLocalStorage, así que interrupt() y task() de la API funcional no pueden ejecutarse aquí. El laboratorio usa la API de grafos y pasa la config a cada llamada a mano. Su segundo proceso es un grafo nuevo sobre el mismo MemorySaver en memoria, y desenchufar aborta el AbortSignal de la ejecución.

## Pruébalo: Fallar, recuperarse, reanudar

Vuelve inestable una herramienta y mira los reintentos, deja caído el servicio de pagos y sigue la ruta hasta la compensación, y luego detén el proceso y reanuda el thread.

https://agenticademy.org/es/paths/advanced-agents/05-loops-that-survive

## Comprobación

charge\_card lanza un 503 en cada llamada. ¿Qué pasa cuando se ejecuta esto?

Código:

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

- **✓** charge\_card se llama dos veces; luego el errorHandler envía la ejecución a release, e invoke devuelve con normalidad.
- charge\_card se llama tres veces, el primer intento y dos reintentos; luego se ejecuta release.
- charge\_card se llama dos veces; luego invoke lanza el 503.
- El errorHandler se ejecuta después del primer 503, y luego se vuelve a intentar charge.

**Por qué:** maxAttempts cuenta todos los intentos, incluido el primero, así que charge\_card se llama dos veces, no tres. El errorHandler solo se ejecuta cuando esos intentos se agotan, nunca entre ellos. Su Command envía la ejecución a release, que termina en END, así que invoke devuelve el estado. Un nodo sin errorHandler hace que invoke lance el 503 cuando se agotan sus intentos.

## Lo que ya puedes hacer

- Dar a un nodo una retryPolicy y un timeout, y predecir cuántos intentos hace.
- Enviar el último fallo de un nodo a un paso de compensación con errorHandler y Command.
- Detener una ejecución con RunControl.requestDrain y reanudarla desde su checkpoint sin repetir los nodos terminados.
- Usar claves de idempotencia para que un nodo que se vuelve a ejecutar tras una caída no actúe dos veces.

---

[Volver a Agentes avanzados](README.md) · [Siguiente: Bucles que se detienen](06-loops-that-stop.md)
