# Tu primer grafo

Construye tus primeros agentes · Capítulo 4 de 8 · 20 minutos

Construir un StateGraph con nodos y aristas, enrutar con una arista condicional y ejecutarlo paso a paso.

## Objetivos

- Conecta START, classify, las dos áreas y END en un grafo que compile.
- Ejecuta el grafo de modo que cada rama se tome una vez.
- Predice cómo se ejecuta el grafo.

## La lectura

### Nodos y aristas

Un StateGraph parte de una definición de estado, como Annotation.Root({ … }) o el MessagesAnnotation del capítulo 3. addNode(name, fn) agrega un nodo: una función que lee el estado y devuelve una actualización, que los reducers del estado combinan. addEdge(a, b) dice que b se ejecuta después de a. START y END son la entrada y la salida del grafo: una arista desde START elige el primer nodo, y una arista hacia END termina la ejecución.

### Ramas

addConditionalEdges(from, router, targets) pone una decisión después de un nodo. El router es una función del estado que devuelve el nombre del siguiente nodo, y targets lista los nodos que puede devolver. Así es como un grafo envía una petición de reembolso a un área y una pregunta a otra. Sin router, dos aristas simples que salen de un mismo nodo se ejecutan las dos.

### Compilar, luego ejecutar

compile() revisa la estructura y devuelve un grafo que puedes invoke() o stream(). Lanza un error si algo está mal, por ejemplo un nodo al que no llega ninguna arista (UnreachableNodeError). No exige que cada nodo llegue a END: un nodo sin arista de salida simplemente termina la ejecución ahí.

Una ejecución avanza en supersteps. En cada uno, todos los nodos que están listos se ejecutan, juntos; luego sus actualizaciones se combinan en el estado, y las aristas deciden qué nodos quedan listos después. stream() informa la actualización de cada nodo a medida que ocurre. Si dos nodos de un mismo superstep escriben la misma clave, esa clave necesita un reducer que las combine, o LangGraph lanza un error.

## Pruébalo: Conecta un grafo

Dibuja las cinco aristas de START a END; LangGraph compila el grafo después de cada una. Luego ejecuta los dos tickets de ejemplo y mira cada superstep.

https://agenticademy.org/es/paths/first-agents/04-your-first-graph

## Comprobación

¿Cómo se ejecuta este grafo?

Código:

```
new StateGraph(State)
  .addNode("a", a).addNode("b", b).addNode("c", c)
  .addEdge(START, "a")
  .addEdge("a", "b").addEdge("a", "c")
  .addEdge("b", END).addEdge("c", END)
  .compile();
```

- a, luego b, luego c: tres supersteps.
- **✓** a, luego b y c juntos en un superstep: dos supersteps.
- a, b y c todos a la vez.
- No compila: un nodo no puede tener dos aristas de salida.

**Por qué:** Cuando a termina, b y c quedan listos, así que se ejecutan en el mismo superstep y sus actualizaciones se combinan.

## Lo que ya puedes hacer

- Construir y compilar un StateGraph con nodos, aristas, START y END.
- Enrutar con addConditionalEdges.
- Explicar los supersteps y cuándo los nodos se ejecutan en paralelo.

---

[Volver a Construye tus primeros agentes](README.md) · [Siguiente: Herramientas y llamadas a herramientas](05-tools-and-tool-calls.md)
