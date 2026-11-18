# El estado a escala

Agentes avanzados · Capítulo 1 de 10 · 20 minutos

Cómo LangGraph.js 1.5.1 combina las escrituras en una clave del estado: campos de StateSchema con y sin reducer, el orden de las escrituras en paralelo, Overwrite y UntrackedValue.

## Objetivos

- Ejecuta dos nodos en paralelo que escriben la misma clave sin reducer y lee el error.
- Agrega un reducer y mira cómo se combinan las dos escrituras, en orden.
- Usa Overwrite para reemplazar un valor con reducer.
- Predice el final de un superstep.

## La lectura

### Un superstep, muchas escrituras

LangGraph ejecuta un grafo en supersteps, como en Sistemas de agentes, capítulo 5. Todos los nodos pendientes se ejecutan en el mismo superstep, en paralelo, y devuelven una actualización: un objeto con las claves del estado que cambian. Cuando todos terminan, LangGraph aplica las actualizaciones y guarda un checkpoint del estado. Los nodos de un superstep nunca ven las actualizaciones de los demás, así que lo que importa es cómo se combinan dos escrituras en la misma clave.

### StateSchema y reducers

Desde LangGraph.js 1.1 puedes declarar el estado con StateSchema, un campo por clave. Un campo dado como un esquema simple, como z.string() de Zod, guarda el último valor escrito y acepta una escritura por superstep. Dos escrituras en él en el mismo superstep fallan con InvalidUpdateError, aunque los valores sean iguales, y no se guarda ningún checkpoint de ese superstep.

Un campo ReducedValue tiene un reducer: una función (current, next) => value que LangGraph llama en cada escritura, por ejemplo para agregar a una lista. La API anterior, Annotation.Root, declara lo mismo con Annotation({ reducer, default }).

### Qué escritura va primero

Un reducer recibe las escrituras de un superstep una por una, en el orden de las rutas de tarea, no en el orden en que terminaron los nodos. Las rutas de tarea se comparan como texto, y la ruta de un nodo iniciado por una arista termina con su nombre, así que aquí reviews va antes que specs. Desde 1.5.0, las ejecuciones en vivo y los checkpointers usan este mismo orden, como LangGraph para Python.

### Overwrite y UntrackedValue

new Overwrite(value), agregado en 1.2, se salta el reducer: la clave pasa a valer value, por ejemplo para reemplazar una lista larga por un resumen. Las demás escrituras en esa clave en el mismo superstep se descartan, y dos escrituras Overwrite en una clave en un mismo superstep fallan.

Un campo UntrackedValue guarda un valor mientras el grafo se ejecuta: los nodos lo leen, pero ningún checkpoint lo guarda, así que una ejecución reanudada desde un checkpoint empieza sin él. Úsalo para datos grandes o temporales que puedes volver a obtener.

### El laboratorio

Halden, una tienda ficticia de artículos para exteriores, pregunta si debe tener una carpa en inventario para el invierno. Dos nodos investigadores, reviews y specs, agregan cada uno una nota a notes en el mismo superstep; después condense resume las notas. Ejecutas este grafo tres veces: sin reducer en notes, con un reducer que agrega y con condense devolviendo un Overwrite.

## Pruébalo: Una línea de tiempo de supersteps

Ejecuta el mismo grafo tres veces: sin reducer, con un reducer y luego con Overwrite. La línea de tiempo muestra qué escribió cada superstep y qué guardó su checkpoint.

https://agenticademy.org/es/paths/advanced-agents/01-state-at-scale

## Comprobación

reviews y specs vienen después de START, así que se ejecutan en el mismo superstep. ¿Qué pasa cuando termina?

Código:

```
const State = new StateSchema({
  topic: z.string(),
  notes: new ReducedValue(z.array(z.string()).default(() => []), {
    reducer: (current, next) => current.concat(next),
  }),
});

new StateGraph(State)
  .addNode("reviews", async () => ({ notes: ["reviews"], topic: "Ridge 2" }))
  .addNode("specs", async () => ({ notes: ["specs"], topic: "Ridge 2" }))
  .addEdge(START, "reviews")
  .addEdge(START, "specs");
```

- notes es ["reviews", "specs"] y topic es "Ridge 2".
- **✓** La ejecución falla con InvalidUpdateError en topic.
- La ejecución falla con InvalidUpdateError en notes.
- topic conserva la escritura del nodo que terminó último.

**Por qué:** topic es un campo simple, así que acepta una escritura por superstep, y recibió dos, aunque las dos son "Ridge 2". notes tiene un reducer, así que sus dos escrituras se combinarían, en el orden de las rutas de tarea. Ningún campo conserva la escritura que terminó última.

## Lo que ya puedes hacer

- Declarar el estado con StateSchema, eligiendo para cada clave un campo simple, un ReducedValue o un UntrackedValue.
- Predecir cómo se combinan las escrituras en paralelo en una clave, en qué orden y cuándo fallan.
- Usar Overwrite para reemplazar un valor con reducer.

---

[Volver a Agentes avanzados](README.md) · [Siguiente: Command y Send](02-command-and-send.md)
