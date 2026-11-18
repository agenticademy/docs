# Mensajes y estado

Construye tus primeros agentes · Capítulo 3 de 8 · 18 minutos

Los cuatro tipos de mensaje, por qué se envía la conversación entera cada vez y cómo LangGraph combina el estado con reducers.

## Objetivos

- Cambia el mensaje de sistema y compara las respuestas.
- Agrega un mensaje y reemplaza otro por su id.
- Predice lo que devuelve el reducer.

## La lectura

### Cuatro tipos de mensaje

Una conversación es una lista de mensajes, y cada mensaje tiene un tipo. SystemMessage contiene tus instrucciones para el modelo. HumanMessage es lo que escribió el usuario. AIMessage es la respuesta del modelo; puede traer tool\_calls, peticiones para ejecutar herramientas. ToolMessage devuelve el resultado de una herramienta, vinculado a su petición por tool\_call\_id. Los cuatro vienen de @langchain/core/messages.

### El modelo no recuerda nada

Un modelo de chat no guarda memoria entre llamadas. Cada llamada envía la lista entera, y la respuesta depende solo de esa lista. Para continuar una conversación, agregas la respuesta y la siguiente pregunta, y vuelves a enviar todo. Por eso también el conteo de tokens de entrada crece con cada vuelta.

### Estado y reducers

En LangGraph, los datos que un grafo lleva de paso a paso son su estado. Un nodo no sobrescribe el estado: devuelve una actualización, y un reducer decide cómo se combina la actualización con lo que ya hay. Sin reducer, el valor nuevo reemplaza al viejo.

Para los mensajes, LangGraph ofrece MessagesAnnotation, un estado con una sola clave, messages, combinada por messagesStateReducer. Cada mensaje recibe un id; el reducer le asigna uno si no lo tiene. Un mensaje con un id nuevo se agrega al final. Un mensaje con un id que ya está en la lista reemplaza a ese mensaje en su lugar. Un RemoveMessage con un id borra ese mensaje. Así es como un grafo edita una conversación, por ejemplo para recortar vueltas viejas, sin reconstruir la lista.

## Pruébalo: La pila de mensajes

Llama al modelo, cambia el mensaje de sistema y llama de nuevo, y luego envía actualizaciones de estado y mira cómo el reducer agrega, reemplaza y quita mensajes.

https://agenticademy.org/es/paths/first-agents/03-messages-and-state

## Comprobación

¿Qué devuelve messagesStateReducer?

Código:

```
messagesStateReducer(
  [new HumanMessage({ id: "a", content: "Hi" }), new AIMessage({ id: "b", content: "Hello" })],
  [new AIMessage({ id: "b", content: "Hello!" }), new HumanMessage({ id: "c", content: "Thanks" })],
);
```

- Cuatro mensajes: a, b, b, c.
- **✓** Tres mensajes: a, b (ahora "Hello!"), c.
- Dos mensajes: b y c.
- Lanza un error porque b aparece dos veces.

**Por qué:** La actualización con id b reemplaza a la b existente en su lugar, y c es nuevo, así que se agrega al final.

## Lo que ya puedes hacer

- Usar bien los cuatro tipos de mensaje.
- Explicar por qué la conversación se envía en cada llamada.
- Predecir cómo el reducer de mensajes de LangGraph combina una actualización.

---

[Volver a Construye tus primeros agentes](README.md) · [Siguiente: Tu primer grafo](04-your-first-graph.md)
