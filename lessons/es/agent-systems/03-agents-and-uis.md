# Agentes e interfaces: AG-UI y A2UI

Sistemas de agentes y estándares · Capítulo 3 de 8 · 25 minutos

Transmitir el trabajo de un agente a una app con AG-UI, y dejar que un agente describa componentes de interfaz seguros con A2UI.

## Objetivos

- Mira el flujo de eventos AG-UI de una ejecución hasta el final.
- Confirma el reembolso mediante una herramienta de front-end.
- Inyecta un componente fuera del catálogo de A2UI y mira cómo se rechaza.
- Supera la comprobación.

## La lectura

### AG-UI: el trabajo del agente como un flujo

MCP conecta un agente con herramientas y A2A con otros agentes. AG-UI (Agent–User Interaction protocol) lo conecta con la app que usa una persona. La app envía un RunAgentInput (id de thread, id de ejecución, mensajes, estado y las herramientas que ofrece la app) y lee de vuelta un flujo de eventos, aquí como server-sent events.

Una ejecución empieza con RUN\_STARTED y termina con RUN\_FINISHED o RUN\_ERROR. El texto llega como TEXT\_MESSAGE\_START, luego fragmentos TEXT\_MESSAGE\_CONTENT, luego TEXT\_MESSAGE\_END. Las llamadas a herramientas llegan igual: TOOL\_CALL\_START, TOOL\_CALL\_ARGS, TOOL\_CALL\_END, y TOOL\_CALL\_RESULT para las herramientas que ejecutó el agente. El estado compartido se mueve como STATE\_SNAPSHOT, el estado entero, o STATE\_DELTA, una lista de operaciones JSON Patch. El cliente oficial, @ag-ui/client 1.0.2, revisa el esquema y el orden de cada evento antes de aplicarlo.

### Herramientas de front-end

Algunas herramientas pertenecen a la app: un botón de confirmar, un selector de fecha. La app las lista en RunAgentInput.tools. Cuando el agente llama a una, la ejecución termina con esa llamada pendiente; la app ejecuta la herramienta, aquí preguntándole al cliente, y empieza la siguiente ejecución con el resultado como mensaje de herramienta. La respuesta de la persona pasa a ser parte de la conversación, y el agente decide qué hacer con ella.

### A2UI: interfaces como datos

A2UI (protocolo v0.9) permite que un agente describa una pieza de interfaz sin enviar código. createSurface abre una superficie y nombra el catálogo que usa; updateComponents envía una lista plana de componentes que se refieren entre sí por id; updateDataModel envía los valores a los que se vinculan. La app renderiza solo componentes de su propio catálogo, aquí siete tipos: Card, Column, Row, List, Text, Button y Divider. Con validación estricta, @a2ui/web\_core 0.12.0 rechaza un tipo desconocido y la superficie queda como estaba, así que un agente, o un prompt que lo engañó, no puede hacer que la app ejecute código nuevo.

### Qué haces aquí

Un cliente pregunta en el chat de soporte por el cobro doble. Por AG-UI, mira el flujo de eventos y luego confirma el reembolso en la tarjeta que dibuja la app. Después cambia el laboratorio a A2UI, donde el agente arma una tarjeta de reembolso a partir de mensajes. Activa Interceptar, retén el mensaje updateComponents y agrega un componente Script: web\_core lo rechaza.

## Pruébalo: Un agente arma una pantalla

Cuatro pasos, una acción cada uno, y luego dos opcionales. El laboratorio debajo de los pasos dibuja cada mensaje como una fila, de arriba abajo. Mientras haya un laboratorio A2UI abierto, un árbol debajo del laboratorio muestra los componentes que envió el agente, unidos por id.

https://agenticademy.org/es/paths/agent-systems/03-agents-and-uis

## Comprobación

### Pregunta 1 de 4

¿Qué es una herramienta de front-end de AG-UI?

- **✓** Una herramienta que ejecuta la app, como un diálogo de confirmación; su resultado vuelve al agente en la siguiente ejecución.
- Una herramienta que dibuja gráficos en el servidor.
- Una extensión del navegador que instala el agente.

**Por qué:** El agente la pide como cualquier otra herramienta, pero la app la declara y la ejecuta, para que una persona pueda participar en el paso. La ejecución termina con la llamada pendiente; el resultado empieza la siguiente ejecución.

### Pregunta 2 de 4

¿Por qué A2UI renderiza desde un catálogo en lugar de ejecutar el markup del agente?

- Es más rápido.
- **✓** La app decide qué componentes existen, así que un agente, o el texto que se le dio, no puede hacer que la app ejecute código nuevo.
- HTML exige catálogos.

**Por qué:** Describir la UI como datos deja el control en la app. Con validación estricta, un tipo de componente fuera del catálogo se rechaza, envíe lo que envíe el agente.

### Pregunta 3 de 4

¿Qué lleva un evento STATE\_DELTA de AG-UI?

- Una copia completa del estado.
- **✓** Operaciones JSON Patch para aplicar al estado compartido.
- El uso de tokens del modelo.

**Por qué:** Los deltas mantienen pequeñas las actualizaciones. Si un patch apunta a una ruta que no existe, el cliente no puede aplicarlo y conserva su estado anterior; un STATE\_SNAPSHOT lo vuelve a sincronizar.

### Pregunta 4 de 4

El cliente recibe TEXT\_MESSAGE\_CONTENT para un messageId que nunca tuvo un TEXT\_MESSAGE\_START. ¿Qué pasa?

- El texto se muestra igual.
- **✓** @ag-ui/client rechaza el evento por estar fuera de orden y detiene la ejecución.
- El agente lo reenvía.

**Por qué:** El cliente revisa el orden de los eventos además de su esquema. Contenido para un mensaje que nunca empezó es un error de protocolo, así que la ejecución se detiene.

## Lo que ya puedes hacer

- Leer un flujo de eventos AG-UI y decir qué hace cada evento.
- Usar una herramienta de front-end para poner a una persona dentro de la ejecución de un agente.
- Describir una superficie A2UI: createSurface, updateComponents y updateDataModel.
- Explicar cómo el catálogo de A2UI protege una app de la UI hecha por agentes.

---

[Volver a Sistemas de agentes y estándares](README.md) · [Siguiente: Agentes que pagan: UCP y AP2](04-agent-commerce.md)
