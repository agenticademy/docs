# Viajar en el tiempo

Agentes avanzados · Capítulo 4 de 10 · 20 minutos

Cada superstep guarda un checkpoint, así que puedes leer la historia de una ejecución, repetirla desde un paso pasado, editar un estado pasado y bifurcar un segundo final.

## Objetivos

- Ejecuta un grafo de cinco nodos en un thread y lee su historia de checkpoints.
- Rebobina hasta el paso 3 y repite desde ahí.
- Edita el estado del paso 3, bifurca y compara los dos finales.
- Ordena los pasos de una bifurcación.

## La lectura

### Un thread es una línea de tiempo

Compila un grafo con un checkpointer, ejecútalo con un thread\_id, y LangGraph guarda un checkpoint de todo el estado después de cada superstep, como en la ruta Sistemas de agentes. Cada checkpoint tiene su propio checkpoint\_id, un step y un source en sus metadatos (el step -1 es la entrada), y un parentConfig que nombra el checkpoint anterior. Enlazados por sus padres, los checkpoints de un thread forman una línea de tiempo, y una línea de tiempo puede ramificarse.

### Leer el pasado

getStateHistory({ configurable: { thread\_id } }) lista todos los checkpoints del thread, del más nuevo al más antiguo. Cada elemento es una instantánea del estado: values, next (los nodos que toca ejecutar), config y metadata. getState con un checkpoint\_id en su config lee uno de ellos. Eso es rebobinar: miras un estado pasado y no cambias nada.

### Repetir y bifurcar

invoke(null, config), con una config que nombra un checkpoint pasado, repite desde él. Los nodos anteriores a ese checkpoint no se vuelven a ejecutar; los posteriores sí, así que un modelo real puede responder distinto la segunda vez. Los checkpoints nuevos crecen en una rama nueva que empieza con una copia cuyo source es fork. Los antiguos se quedan.

updateState(config, values, asNode) cambia el pasado sin borrarlo. Escribe un checkpoint nuevo, con source update, encima del que nombra config, como si el nodo asNode hubiera devuelto values, y devuelve la config del checkpoint nuevo. Ejecuta invoke(null, thatConfig) y el thread se bifurca: el primer final sigue en la historia y un segundo final crece desde tu edición. getState sin checkpoint\_id lee el checkpoint más nuevo, que ahora es el final de la bifurcación.

### Durabilidad

La opción durability de una ejecución decide cuándo se escriben los checkpoints. "async", el valor por defecto, guarda cada uno mientras corre el siguiente superstep; "sync" lo guarda antes de que empiece el siguiente superstep; "exit" guarda solo cuando termina la ejecución. "exit" es el que menos escribe, pero no deja ningún paso pasado al que rebobinar, ni nada desde donde reanudar si el proceso se cae a la mitad.

### En esta práctica

La revisión de reembolso de Halden para el ticket T-48213 ejecuta cinco nodos seguidos: read\_ticket, classify, list\_charges, decide y draft\_reply. LangGraph.js 1.5.1 y su MemorySaver se ejecutan en esta página, y classify, decide y draft\_reply consultan a un modelo con guion. La versión para navegador de LangGraph.js no tiene AsyncLocalStorage, así que cada nodo le pasa su config al modelo de forma explícita.

## Pruébalo: Un riel de checkpoints

Tres pasos: ejecuta el grafo y lee sus checkpoints, rebobina hasta el paso 3 y repite, y luego edita el paso 3, bifurca y compara los dos finales.

https://agenticademy.org/es/paths/advanced-agents/04-time-travel

## Comprobación

Ordena los pasos para bifurcar una ejecución desde un checkpoint pasado.

1. Ejecuta el grafo con un checkpointer y un thread\_id.
2. Lista los checkpoints del thread con getStateHistory.
3. Elige por su checkpoint\_id el checkpoint al que quieres volver.
4. Llama a updateState con la config de ese checkpoint y los valores nuevos.
5. Llama a invoke(null, config) con la config que devolvió updateState.
6. Compara el final de la bifurcación con el primero, que sigue en la historia.

**Por qué:** Los checkpoints solo existen una vez que una ejecución en un thread los ha guardado, y encuentras uno pasado en getStateHistory por su checkpoint\_id. updateState necesita la config de ese checkpoint y devuelve la config del checkpoint nuevo que escribe; invoke(null, …) con esa config ejecuta la bifurcación. Nada se sobrescribe, así que el primer final sigue ahí para comparar.

## Lo que ya puedes hacer

- Leer los checkpoints de un thread con getStateHistory, y uno de ellos con getState y su checkpoint\_id.
- Repetir una ejecución desde un checkpoint pasado, y decir qué nodos se vuelven a ejecutar.
- Editar un estado pasado con updateState, bifurcar el thread y comparar los dos finales.
- Elegir un modo de durabilidad sabiendo a qué te deja rebobinar cada uno.

---

[Volver a Agentes avanzados](README.md) · [Siguiente: Bucles que sobreviven](05-loops-that-survive.md)
