# Command y Send

Agentes avanzados · Capítulo 2 de 10 · 20 minutos

Un nodo puede actualizar el estado y elegir el siguiente nodo en un solo paso con Command, y repartir el trabajo entre workers en paralelo con Send, tantos como pida el modelo.

## Objetivos

- Ejecuta un nodo router que actualiza el estado y salta con Command.
- Reparte con Send en tantas revisiones como elija el modelo con guion, para dos tickets.
- Reanuda la ejecución: el reducer ya reunió los resultados, y un nodo los resume.
- Predice con qué se ejecuta el nodo que reúne.

## La lectura

### Command: actualizar y enrutar en un solo return

Un nodo suele devolver una actualización del estado, y las aristas del grafo deciden qué se ejecuta después. En su lugar, un nodo puede devolver new Command({ update, goto }). update se escribe en el estado como lo haría un return normal, y goto nombra el siguiente nodo, elegido mientras el nodo se ejecuta. Un router puede clasificar un ticket, guardar la clase y saltar al nodo correcto en un solo paso.

Declara a dónde puede ir un Command con ends: addNode("triage", triage, { ends: ["plan", "reply"] }). compile() usa ends para comprobar que se pueda llegar a cada nodo: aquí plan y reply no tienen otra entrada, así que sin ends lanza UnreachableNodeError. getGraphAsync() dibuja ends como aristas. Un goto se suma a las aristas estáticas del nodo; no las reemplaza, así que un nodo que devuelve Command no suele tener ninguna.

### Send: tantos workers como pida el modelo

new Send("check", { ticket, check }) inicia una ejecución de un nodo, llamada tarea, con su propia entrada en lugar del estado del grafo. Una arista condicional que devuelve una lista de Sends reparte el trabajo: todas esas tareas se ejecutan en el siguiente superstep, al mismo tiempo. La lista puede venir del modelo, así que el ancho se decide en tiempo de ejecución: tres revisiones para un ticket, cinco para otro. El goto de Command también acepta Sends.

### Reunir los resultados

Todas las tareas escriben la misma clave, así que esa clave necesita un reducer, como en el capítulo anterior. Al final del superstep, el reducer combina sus escrituras en el orden de las tareas, que es el orden de los Sends, sin importar en qué orden terminaron las tareas. Un nodo con una arista desde los workers se ejecuta entonces una vez, en el siguiente superstep, y las lee todas. Eso es map-reduce en un grafo.

### Traspasos, y este laboratorio

Dentro de un subgrafo, new Command({ graph: Command.PARENT, goto }) salta a un nodo del grafo padre. Así un agente le pasa trabajo a otro; el próximo capítulo arma equipos con eso.

En el laboratorio, la mesa de tickets de Halden, la tienda ficticia del capítulo anterior, ejecuta un solo grafo: triage enruta cada ticket, plan deja que el modelo con guion elija las revisiones, y summarise escribe la respuesta. La página pausa las ejecuciones con interruptAfter e interruptBefore, pasados a stream(), porque interrupt() necesita AsyncLocalStorage, que la versión para navegador de LangGraph.js no tiene.

## Pruébalo: Enrutar, repartir, reunir

Tres pasos sobre un mismo grafo de LangGraph.js: enruta un ticket con Command, reparte en las revisiones que elige el modelo y luego reanuda la ejecución para reunir y resumir.

https://agenticademy.org/es/paths/advanced-agents/02-command-and-send

## Comprobación

El modelo con guion eligió tres revisiones, y cada revisión tarda un tiempo distinto. ¿Cuántas veces se ejecuta summarise, y qué escribe?

Código:

```
// findings: a ReducedValue whose
// reducer concatenates lists.
// plan returns { checks:
//   ["order", "shipping", "stock"] }
new StateGraph(State)
  .addNode("plan", plan)
  .addNode("check", async ({ check }) =>
    ({ findings: [check] }))
  .addNode("summarise", (s) => ({
    answer: s.findings.join(" + "),
  }))
  .addEdge(START, "plan")
  .addConditionalEdges("plan", (s) =>
    s.checks.map((c) =>
      new Send("check", { check: c })),
    ["check"])
  .addEdge("check", "summarise")
  .addEdge("summarise", END)
  .compile();
```

- Tres veces, una por cada revisión terminada: primero un nombre, luego dos, luego los tres.
- **✓** Una vez, después de las tres: "order + shipping + stock", en el orden de los Sends.
- Una vez, con los tres nombres en el orden en que terminaron las revisiones, así que puede cambiar de una ejecución a otra.
- Una vez: "stock". La escritura de cada revisión reemplaza a la anterior.

**Por qué:** Los tres Sends inician tres tareas check en el mismo superstep. Cuando termina, el reducer combina sus escrituras en el orden de las tareas, que es el orden de los Sends, no el orden en que terminaron. summarise tiene una arista desde check, así que se ejecuta una vez, en el siguiente superstep, y lee las tres. Sin el reducer, tres escrituras a findings en un mismo superstep fallarían.

## Lo que ya puedes hacer

- Enrutar y actualizar el estado en un solo paso devolviendo Command desde un nodo, con sus destinos declarados en ends.
- Repartir con Send en un ancho que el modelo elige en tiempo de ejecución.
- Reunir resultados en paralelo con un reducer, y predecir su orden y cuándo se ejecuta el siguiente nodo.

---

[Volver a Agentes avanzados](README.md) · [Siguiente: Subgrafos y equipos](03-subgraphs-and-teams.md)
