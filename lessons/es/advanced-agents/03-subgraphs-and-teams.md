# Subgrafos y equipos

Agentes avanzados · Capítulo 3 de 10 · 20 minutos

Arma equipos de agentes con subgrafos, decide cómo comparten el estado, lee sus namespaces en el stream y compara patrones multiagente por llamadas al modelo y tokens.

## Objetivos

- Ejecuta un equipo de equipos y lee los namespaces de su stream.
- Pausa dentro de un equipo y abre su propio estado.
- Compara subagentes con traspasos en llamadas al modelo y tokens.
- Predice lo que emite un subgrafo en el stream.

## La lectura

### Subgrafos

Un grafo compilado puede ser un nodo de otro grafo: un subgrafo. Cuando los dos comparten una clave de estado, el subgrafo compilado es el nodo, como en addNode("research\_team", researchTeam). Lee las claves que comparte con el padre, lo que escribe en ellas vuelve, y las claves que solo él tiene se quedan dentro.

Sin ninguna clave compartida, llama al subgrafo desde un nodo envoltorio que convierte el estado del padre en la entrada del subgrafo, y su salida de vuelta en una actualización. Pasa { subgraphs: [team] } a addNode para que getGraphAsync y getState puedan seguir encontrándolo.

### Namespaces y streaming

Cada ejecución de un subgrafo guarda sus checkpoints en su propio namespace dentro del hilo del padre, nombrado según su nodo y su tarea: research\_team:<task id>. stream(input, { streamMode: "updates", subgraphs: true }) produce pares [namespace, update], con [] para el grafo superior. getGraphAsync({ xray: true }) dibuja los nodos propios de los subgrafos, como research\_team:lead.

Mientras una ejecución está pausada dentro de un subgrafo, getState(config, { subgraphs: true }) también devuelve el estado propio del subgrafo. En un navegador interrupt() no puede ejecutarse, porque necesita AsyncLocalStorage, así que este laboratorio pausa con interruptBefore y pasa config a cada subgrafo a mano.

### Equipos de equipos y traspasos

Un supervisor puede dirigir a otros supervisores. En el laboratorio un jefe de soporte enruta con Command({ goto }) a dos equipos, y el líder de cada equipo reparte partes del trabajo entre sus workers. Un nodo dentro de un subgrafo también puede pasar el control a un nodo del padre: new Command({ graph: Command.PARENT, goto: "writer", update }). Eso es un traspaso: el siguiente agente toma el control con el historial que pusiste en update.

### Elegir un patrón

La documentación de JS de LangChain nombra cinco patrones multiagente. Subagentes: un agente principal llama a otros agentes como herramientas y recibe cada resultado. Traspasos: cambia el agente a cargo. Skills: un agente carga prompts de especialista cuando los necesita. Router: un paso clasifica la entrada y la envía a agentes. Flujo personalizado: tu propio grafo de LangGraph. El capítulo 5 de Sistemas de agentes llamó traspasos a sus llamadas transfer\_to\_<agent>, pero su supervisor usaba subagentes: cada resultado volvía a él.

La documentación los compara (consultada el 2026-11-04): una solicitud de un solo paso toma 4 llamadas al modelo con subagentes y 3 con traspasos; una pregunta sobre tres dominios toma 5 llamadas y unos 9K tokens con subagentes, y 7+ llamadas y unos 14K+ con traspasos. En el laboratorio ejecutas un mismo trabajo de las dos formas y cuentas.

## Pruébalo: Un equipo de equipos

Tres pasos: ejecuta un equipo de equipos y lee su stream, pausa dentro de un equipo para abrir su estado, y luego ejecuta el mismo trabajo como subagentes y como traspasos.

https://agenticademy.org/es/paths/advanced-agents/03-subgraphs-and-teams

## Comprobación

El equipo comparte messages con el grafo superior; notes es su propia clave. ¿Qué imprime el bucle?

Código:

```
const Team = Annotation.Root({
  ...MessagesAnnotation.spec,
  notes: Annotation({ reducer: (a, b) => a.concat(b), default: () => [] }),
});
const team = new StateGraph(Team)
  .addNode("work", () => ({ notes: ["n1"], messages: [{ role: "assistant", content: "done" }] }))
  .addEdge(START, "work")
  .compile();
const top = new StateGraph(MessagesAnnotation)
  .addNode("team", team)
  .addEdge(START, "team")
  .compile();

const input = { messages: [{ role: "user", content: "go" }] };
for await (const [ns, update] of await top.stream(input, { streamMode: "updates", subgraphs: true })) {
  const [node, values] = Object.entries(update)[0];
  console.log(ns.length ? "inside" : "top", node, Object.keys(values));
}
```

- **✓** inside work ["notes", "messages"], luego top team ["messages"]: primero la actualización propia del equipo, luego lo que vuelve al grafo superior.
- top team ["notes", "messages"]: el stream muestra solo el grafo superior, y las claves del subgrafo vuelven con él.
- inside work ["notes", "messages"], luego top team ["notes", "messages"]: vuelve cada clave que escribió el equipo.
- Lanza un error: el grafo superior no tiene la clave notes, así que el equipo no puede escribirla.

**Por qué:** Con subgraphs: true el stream también produce las actualizaciones propias del subgrafo, bajo su namespace ["team:<task id>"]; sin eso verías solo la línea top. El equipo es el nodo mismo, así que las claves que comparte vuelven: messages. notes existe solo dentro del equipo, así que se queda ahí, y escribirla ahí está permitido.

## Lo que ya puedes hacer

- Agregar un subgrafo como nodo cuando comparte claves, o llamarlo desde un nodo envoltorio que convierte el estado.
- Hacer streaming con subgraphs: true y leer el namespace de cada actualización; leer el estado de un subgrafo pausado con getState.
- Pasar el control a un nodo del grafo padre con Command.PARENT, y construir supervisores de supervisores.
- Elegir entre subagentes, traspasos, skills, un router y un flujo personalizado según sus llamadas al modelo y tokens.

---

[Volver a Agentes avanzados](README.md) · [Siguiente: Viajar en el tiempo](04-time-travel.md)
