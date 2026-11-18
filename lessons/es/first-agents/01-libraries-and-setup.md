# Las librerías, y qué se ejecuta aquí

Construye tus primeros agentes · Capítulo 1 de 8 · 15 minutos

Qué paquetes de LangChain.js y LangGraph.js necesita un agente, qué hace cada uno y cómo se ejecutan en un navegador.

## Objetivos

- Abre los seis paquetes del mapa y lee lo que te da cada uno.
- Ejecuta el hello-world en esta página y lee la respuesta.
- Supera la comprobación.

## La lectura

### Los paquetes

LangChain.js y LangGraph.js son librerías de JavaScript de código abierto para construir con modelos de lenguaje. Vienen como paquetes separados, así que instalas solo lo que usas.

@langchain/core contiene las piezas compartidas: los mensajes, la interfaz de modelo de chat, las herramientas y los callbacks. @langchain/langgraph es el runtime para agentes como grafos: estado, nodos, aristas, checkpoints y pausas para aprobación. Un paquete de proveedor como @langchain/anthropic o @langchain/openai te da la clase de modelo de chat para la API de un proveedor; la misma clase ChatOpenAI también habla con servicios compatibles con OpenAI como OpenRouter, Hugging Face y NVIDIA. zod describe y valida la entrada de tus herramientas; LangChain convierte un esquema de zod en el JSON Schema que lee el modelo. langsmith es opcional y registra trazas de cada ejecución.

### Por qué una librería de grafos

Puedes escribir un ciclo de agente a mano: llamas al modelo, ejecutas las herramientas que pide y repites. El capítulo 6 construye ese ciclo como un grafo de dos nodos. LangGraph agrega lo que le falta a un ciclo escrito a mano: estado guardado después de cada paso, para que una ejecución pueda pausarse por una persona y reanudarse después; ramas que se ejecutan en paralelo; y una estructura que puedes inspeccionar y dibujar.

El paquete langchain se apoya en LangGraph y ofrece createAgent, un agente listo para usar con middleware. Esta ruta primero construye el grafo a mano, para que después sepas qué hacen por ti esos atajos.

### Instalación

En tu propio proyecto: npm install @langchain/core @langchain/langgraph @langchain/anthropic zod. Esta ruta usa @langchain/core 1.2.18, @langchain/langgraph 1.5.1 y zod 4.6.7. Fija versiones exactas en proyectos reales: estas librerías cambian rápido.

### Qué cambia en un navegador

Todo en esta ruta se ejecuta en tu navegador, así que importa LangGraph desde @langchain/langgraph/web. El punto de entrada principal pasa la configuración a las llamadas anidadas con AsyncLocalStorage, que los navegadores no tienen. Con /web pasas config explícitamente a las llamadas anidadas y pausas las ejecuciones con breakpoints (interruptBefore) en lugar de interrupt(). El resto de la API es igual.

El modelo sigue un guion por defecto, así que no necesitas clave y cada ejecución da el mismo resultado. En este capítulo es el propio FakeListChatModel de LangChain, que devuelve las respuestas que le das. Desde el capítulo 2 puedes conectar un modelo real con tu propia clave.

## Pruébalo: Los paquetes

Abre cada paquete del mapa para ver lo que te da, la versión que se usa aquí y cómo importarlo. Luego ejecuta el hello-world.

https://agenticademy.org/es/paths/first-agents/01-libraries-and-setup

## Comprobación

### Pregunta 1 de 4

¿Qué paquete define HumanMessage, AIMessage y tool()?

- @langchain/langgraph
- **✓** @langchain/core
- @langchain/anthropic
- zod

**Por qué:** @langchain/core contiene las piezas compartidas. LangGraph y los paquetes de proveedor las importan de ahí.

### Pregunta 2 de 4

Ejecutas un grafo de LangGraph en un navegador. ¿Desde dónde importas StateGraph?

- @langchain/langgraph
- langchain
- **✓** @langchain/langgraph/web

**Por qué:** El punto de entrada /web no depende de AsyncLocalStorage, que los navegadores no tienen. Entonces pasas config explícitamente a las llamadas anidadas.

### Pregunta 3 de 4

¿Qué hace zod en un agente de LangChain?

- **✓** Describe y valida las entradas de las herramientas; LangChain le envía el esquema al modelo como JSON Schema.
- Llama al modelo.
- Guarda checkpoints entre pasos.

**Por qué:** El esquema sirve dos veces: el modelo lo lee para saber qué argumentos enviar, y tu código lo usa para rechazar argumentos inválidos.

### Pregunta 4 de 4

¿Por qué el hello-world usa FakeListChatModel?

- Razona mejor que los modelos reales.
- Los modelos reales no se pueden llamar desde un navegador.
- **✓** Devuelve respuestas fijas, así que el código se ejecuta sin clave y sin red, igual cada vez.

**Por qué:** Un modelo falso hace que el código sea testeable y gratis de ejecutar. Los modelos reales se pueden llamar desde un navegador con una clave, como muestra el capítulo 2.

## Lo que ya puedes hacer

- Elegir los paquetes para un agente de LangGraph.js y decir qué hace cada uno.
- Instalarlos con versiones fijas.
- Explicar qué cambia cuando LangGraph se ejecuta en un navegador.
- Ejecutar código de LangChain con un modelo falso, sin necesidad de clave.

---

[Volver a Construye tus primeros agentes](README.md) · [Siguiente: Una llamada al modelo](02-one-model-call.md)
