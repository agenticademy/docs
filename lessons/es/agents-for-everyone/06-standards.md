# Estándares, con palabras simples

Agentes para todos · Capítulo 6 de 8 · 14 minutos

Los estándares son formatos de mensaje acordados que permiten que agentes, herramientas, apps y tiendas hechos por distintas personas trabajen juntos.

## Objetivos

- Conecta el estándar correcto en cada conexión de un recorrido.
- Abre un mensaje real y lee qué significan sus campos.
- Supera la comprobación.

## La lectura

### Por qué estándares

Un estándar es una forma acordada de escribir mensajes, algo así como la forma de un enchufe. Si todos fabrican con la misma forma, cualquier enchufe entra en cualquier toma. Sin estándares, cada agente necesitaría su propio cableado a medida para cada app, herramienta y otro agente con el que trabaje. Con ellos, una empresa construye su lado una vez y puede conectarse con muchas otras.

Un mensaje aquí es un texto pequeño y estructurado que un programa le envía a otro: para quién es, qué se pide y los detalles. Verás mensajes reales en la simulación.

### Cuatro tipos de conexión

Entre la app de una persona y un agente: AG-UI (Agent–User Interaction Protocol) lleva la conversación, incluida la respuesta a medida que se escribe. A2UI (Agent-to-User Interface) permite que un agente describa una pantalla, como un formulario, para que la app la dibuje.

Entre agentes: A2A (Agent2Agent Protocol) permite que un agente le pase una tarea a otro, incluso a uno de otra empresa, y la siga hasta que esté hecha. ANP (Agent Network Protocol) es para agentes que se encuentran en la web abierta.

Entre un agente y sus herramientas: MCP (Model Context Protocol) permite que un agente encuentre y use herramientas que gestiona otra persona, como la búsqueda de pedidos de una mesa de ayuda.

Entre un agente y tiendas o pagos: UCP (Universal Commerce Protocol) les da a los agentes una forma común de pagar en una tienda, y AP2 (Agent Payments Protocol) permite que un agente pague solo con una prueba de que estuviste de acuerdo.

### Qué es un estándar, y qué no

Un estándar no es un producto, una app ni una empresa. Es una descripción escrita de mensajes, y cualquiera puede construir siguiéndola. Estos estándares son jóvenes: el primero, MCP, apareció a fines de 2024, y la mayoría son más nuevos. Todavía cambian, así que cuando leas sobre uno, revisa la fecha y la versión.

## Pruébalo: Conecta los estándares

Conecta el recorrido enlace por enlace, cada uno con el estándar hecho para él. Después reproduce el recorrido y abre un mensaje real.

https://agenticademy.org/es/paths/agents-for-everyone/06-standards

## Comprobación

### Pregunta 1 de 3

¿Qué estándar conecta un agente con sus herramientas?

- A2A
- **✓** MCP
- AG-UI

**Por qué:** MCP, el Model Context Protocol, es la forma en que un agente encuentra y llama herramientas que gestiona otra persona. A2A conecta agentes con agentes, y AG-UI conecta una app con un agente.

### Pregunta 2 de 3

¿Para qué sirve A2A?

- **✓** Para que un agente le pida a otro agente, de otra persona, que haga una tarea.
- Para dibujar botones en una app.
- Para pagar en una tienda.

**Por qué:** A2A (Agent2Agent) permite que un agente le pase una tarea a otro y la siga hasta que esté hecha. Dibujar pantallas es trabajo de A2UI, y pagar, de AP2.

### Pregunta 3 de 3

¿Qué es un estándar, en este sentido?

- Un producto que compras.
- **✓** Un formato acordado para mensajes que cualquiera puede seguir.
- Una ley sobre la IA.

**Por qué:** Como la forma de un enchufe, un estándar dice cómo son los mensajes, para que piezas hechas por distintas personas encajen. No es un producto, y no es una ley.

## Lo que ya puedes hacer

- Explicar qué es un estándar y por qué los agentes los necesitan.
- Nombrar el estándar de cada tipo de conexión: apps, agentes, herramientas, pagos.
- Leer, con ayuda, los campos principales de un mensaje real entre agentes.

---

[Volver a Agentes para todos](README.md) · [Siguiente: Riesgos, seguridad y decir que sí](07-risks-and-approval.md)
