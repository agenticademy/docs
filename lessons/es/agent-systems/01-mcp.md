# MCP: herramientas desde un servidor

Sistemas de agentes y estándares · Capítulo 1 de 8 · 25 minutos

Cómo un agente encuentra y llama herramientas en un servidor MCP, y cómo el protocolo 2026-07-28 le pregunta a una persona antes de una llamada riesgosa.

## Objetivos

- Ejecuta el escenario hasta que se emita el reembolso.
- Responde tú la confirmación de la mesa de ayuda.
- Altera un mensaje y mira cómo el SDK real lo rechaza.
- Supera la comprobación.

## La lectura

### Hosts, clientes y servidores

El Model Context Protocol (MCP) es un estándar abierto para conectar aplicaciones de IA con herramientas y datos. La aplicación, llamada host, ejecuta un cliente MCP por cada servidor que usa. Un servidor ofrece tools (acciones que el modelo puede llamar), resources (datos que la aplicación puede leer) y prompts (plantillas que una persona puede elegir). Los mensajes son JSON-RPC 2.0, sobre HTTP o sobre la entrada y salida estándar de un proceso local (stdio).

En el agente de la ruta anterior, las herramientas eran funciones en tu código. Con MCP viven en un servidor que otra persona puede escribir y ejecutar, y cualquier host MCP puede usarlas.

### Qué cambió en 2026-07-28

Este capítulo usa la versión de protocolo 2026-07-28 con el SDK oficial de TypeScript v2. Verás tres cambios en el cable.

Descubrimiento: el cliente llama a server/discover para conocer las versiones y capacidades del servidor. Los servidores con las versiones de 2025 responden initialize en su lugar, y el SDK recurre a eso.

Una versión en cada petición: cada petición lleva su versión de protocolo en \_meta, así que el servidor no guarda sesión, y cualquier instancia suya puede responder cualquier petición.

Input requerido: cuando una herramienta necesita la respuesta de una persona, el servidor termina la petición con input\_required, la pregunta y un requestState firmado. El cliente le pregunta a la persona y luego llama de nuevo a la herramienta con la respuesta y el mismo requestState. El servidor verifica la firma, así que el reintento debe coincidir con lo que vio la persona.

### Las herramientas son un contrato

El modelo ve solo el nombre, la descripción y el JSON Schema de una herramienta. Las anotaciones como readOnlyHint y destructiveHint son pistas del servidor: un cliente confía en ellas solo en la medida en que confía en ese servidor. El reembolso de este laboratorio es seguro porque el propio servidor pide una confirmación y la verifica, no porque el modelo se porte bien.

### Qué haces aquí

El agente de soporte de Halden, un agente de LangChain con el cliente del SDK, y la mesa de ayuda de Halden, el servidor del SDK, se ejecutan los dos en esta página. Ejecuta el escenario y responde tú la pregunta del reembolso. Luego activa Interceptar, retén un mensaje y altéralo: el código real del SDK lo rechaza.

## Pruébalo: El cable: un agente y una mesa de ayuda MCP

Cuatro pasos, una acción cada uno, y un quinto opcional. El diagrama debajo de los pasos tiene una columna por participante y una fila por mensaje, en el orden en que se envía, de arriba abajo.

https://agenticademy.org/es/paths/agent-systems/01-mcp

## Comprobación

### Pregunta 1 de 4

En MCP, ¿quién ejecuta el cliente?

- El servidor.
- **✓** La aplicación host, por ejemplo el agente de soporte.
- El proveedor del modelo.

**Por qué:** El host, como un agente o un IDE, ejecuta un cliente por cada servidor al que se conecta. El modelo nunca habla directamente con el servidor; lo hace el host, cuando el modelo pide una herramienta.

### Pregunta 2 de 4

La mesa de ayuda necesita el visto bueno del responsable de soporte antes de un reembolso. ¿Qué envía un servidor 2026-07-28?

- Una petición nueva del servidor al cliente, pidiendo input.
- **✓** Un resultado con input\_required, la pregunta y un requestState firmado, que termina la petición.
- El error -32602.

**Por qué:** El servidor termina la petición en lugar de esperar con una conexión abierta. El cliente le pregunta a la persona y reintenta con la respuesta y el mismo requestState, así que cualquier instancia del servidor puede terminar el trabajo.

### Pregunta 3 de 4

En el reintento, subes el monto del reembolso. ¿Por qué lo rechaza el servidor?

- El modelo nota el cambio.
- **✓** Los argumentos ya no coinciden con el requestState firmado, que se emitió para lo que aprobó la persona.
- MCP no permite montos en los argumentos.

**Por qué:** El requestState lo firma el servidor (aquí con HMAC). El servidor lee de vuelta el pedido y el monto aprobados y rechaza cualquier llamada que difiera.

### Pregunta 4 de 4

Un servidor marca una herramienta con readOnlyHint: true. ¿Qué debería hacer el cliente?

- Ejecutarla sin preguntarle nunca a nadie, ya que no puede cambiar nada.
- **✓** Tratarlo como una pista, confiable solo en la medida en que el cliente confía en ese servidor.
- Negarse a ejecutarla.

**Por qué:** Las anotaciones describen lo que afirma el servidor. Un servidor malicioso o con errores puede etiquetar mal una herramienta, así que la política propia del cliente decide qué necesita aprobación.

## Lo que ya puedes hacer

- Describir un host, un cliente y un servidor MCP, y las tres cosas que ofrece un servidor.
- Seguir el flujo de 2026-07-28: server/discover, tools/list, tools/call, input\_required y el reintento.
- Explicar por qué un requestState firmado mantiene una confirmación atada a lo que vio la persona.
- Tratar las anotaciones de herramientas como pistas, no como garantías.

---

[Volver a Sistemas de agentes y estándares](README.md) · [Siguiente: A2A: agentes que delegan en agentes](02-a2a.md)
