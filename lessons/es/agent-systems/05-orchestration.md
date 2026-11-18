# Orquestación: supervisores, fan-out, subgrafos

Sistemas de agentes y estándares · Capítulo 5 de 8 · 25 minutos

Patrones para varios agentes en un solo grafo de LangGraph: un supervisor que enruta, Send para trabajo en paralelo, subgrafos y el checkpoint que guarda cada superstep.

## Objetivos

- Mira a dos especialistas ejecutarse en el mismo superstep.
- Abre la ejecución de un subgrafo anidado.
- Ejecuta el mismo trabajo con un agente y con un supervisor.
- Predice el fan-out.

## La lectura

### Supersteps

LangGraph ejecuta un grafo en supersteps. En cada superstep, todos los nodos que tocan se ejecutan, en paralelo; luego sus actualizaciones se combinan en el estado con el reducer de cada clave, y las aristas deciden qué nodos se ejecutan después. Los mensajes usan un reducer que agrega al final, así que dos nodos pueden agregar mensajes en un mismo superstep.

### Hilos y checkpoints

Compila un grafo con un checkpointer, como MemorySaver, y pasa un thread\_id al ejecutarlo. LangGraph guarda un checkpoint de todo el estado bajo ese hilo: uno para la entrada y luego uno al final de cada superstep. getState({ configurable: { thread\_id } }) lee el último: los valores, el número de paso y next, los nodos que faltan por ejecutarse. Una ejecución posterior en el mismo hilo empieza desde ese estado.

### Supervisor, fan-out, subgrafos

Un supervisor es un agente que decide qué especialista trabaja después. Aquí un traspaso es una llamada a herramienta como transfer\_to\_customer\_support, así que el modelo lo elige como cualquier otra herramienta.

El fan-out ejecuta trabajo en paralelo. Una arista condicional puede devolver una lista de objetos Send en lugar de nombres de nodos. Cada Send inicia una ejecución de un nodo con su propia entrada, todas en el siguiente superstep, y el reducer combina lo que devuelven.

Un subgrafo es un grafo compilado usado dentro de un nodo. Tiene su propio estado y su propio namespace de checkpoints, y sus ejecuciones se anidan bajo el nodo que lo llamó. En este laboratorio el Financial Analyst llama a un subgrafo Data Engineer propio. Los traspasos con Command y los subgrafos que comparten estado llegan más adelante, en la ruta Agentes avanzados.

### Cuándo basta un agente

Cada agente agrega llamadas al modelo, checkpoints y traspasos, y un lugar más donde fallar. Divide el trabajo cuando sus partes son independientes, cuando un solo contexto crecería demasiado, o cuando una parte necesita otras herramientas o instrucciones. Si no, empieza con un agente.

En este laboratorio, el equipo de soporte de Halden atiende el ticket T-48213, un cliente al que le cobraron dos veces el pedido 48213. Ejecútalo de las dos formas y compara lo que miden las trazas.

## Pruébalo: Un cielo de agentes

Tres pasos: ejecuta el trabajo como supervisor y mira cómo cada superstep guarda un checkpoint, abre un subgrafo en el árbol de ejecuciones, y luego ejecuta el mismo trabajo con un agente y compara.

https://agenticademy.org/es/paths/agent-systems/05-orchestration

## Comprobación

results tiene un reducer que concatena listas. ¿Qué devuelve el grafo?

Código:

```
new StateGraph(State)
  .addNode("lookup", s => ({ results: [s.id * 10] }))
  .addConditionalEdges(START, () => [new Send("lookup", { id: 1 }), new Send("lookup", { id: 2 })], ["lookup"])
  .addEdge("lookup", END)
  .compile()
  .invoke({});
```

- **✓** { results: [10, 20] }: dos ejecuciones de lookup en un superstep.
- { results: [20] }: el segundo Send sobrescribe al primero.
- { results: [10] }: solo se ejecuta el primer Send.
- Lanza un error: no se puede enviar dos veces a un nodo.

**Por qué:** Cada Send inicia su propia ejecución de lookup con su propia entrada ({ id: 1 } y { id: 2 }). Las dos se ejecutan en el mismo superstep, y el reducer concatena sus actualizaciones.

## Lo que ya puedes hacer

- Construir un supervisor que enruta a agentes especialistas con llamadas a herramientas de traspaso.
- Hacer fan-out con Send y combinar resultados con un reducer.
- Usar un grafo compilado como subgrafo, y decir cuándo basta un agente.
- Ejecutar un grafo en un hilo y leer su último checkpoint con getState.

---

[Volver a Sistemas de agentes y estándares](README.md) · [Siguiente: Ingeniería de contexto y recuperación](06-context-engineering.md)
