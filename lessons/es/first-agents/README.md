# Construye tus primeros agentes

Principiante · Para desarrolladores · 8 capítulos · 152 min

Para desarrolladores que empiezan con agentes. Construyes paso a paso un pequeño agente de soporte: una llamada al modelo, mensajes y estado, un grafo, herramientas, el ciclo del agente, callbacks con aprobación humana y guardrails. Cada capítulo ejecuta las librerías reales en tu navegador con un modelo con guion, así que no necesitas clave; puedes conectar tu propio modelo cuando quieras.

## Antes de empezar

Algo de JavaScript: funciones, objetos, async/await e instalar paquetes con npm.

## Al final podrás

- Elegir los paquetes de LangChain.js y LangGraph.js para un agente, y saber qué hace cada uno.
- Llamar a un modelo de chat y trabajar con mensajes y con el estado del grafo.
- Construir un StateGraph con nodos, aristas y enrutamiento condicional.
- Definir herramientas con esquemas de zod y ejecutar un agente ReAct mínimo.
- Pausar una ejecución para la aprobación humana con checkpoints e interruptBefore, y registrarla con callbacks.
- Agregar guardrails: validaciones de entrada y salida, listas de herramientas permitidas y límites de pasos.

## Capítulos

1. [Las librerías, y qué se ejecuta aquí](01-libraries-and-setup.md): Qué paquetes de LangChain.js y LangGraph.js necesita un agente, qué hace cada uno y cómo se ejecutan en un navegador.
2. [Una llamada al modelo](02-one-model-call.md): Llamar a un modelo de chat con LangChain.js, leer el AIMessage que devuelve y usar tu propia clave de forma segura.
3. [Mensajes y estado](03-messages-and-state.md): Los cuatro tipos de mensaje, por qué se envía la conversación entera cada vez y cómo LangGraph combina el estado con reducers.
4. [Tu primer grafo](04-your-first-graph.md): Construir un StateGraph con nodos y aristas, enrutar con una arista condicional y ejecutarlo paso a paso.
5. [Herramientas y llamadas a herramientas](05-tools-and-tool-calls.md): Definir herramientas con zod, vincularlas a un modelo y manejar las llamadas a herramientas que hace el modelo.
6. [El ciclo del agente: un agente ReAct mínimo](06-the-agent-loop.md): Un nodo de modelo, un nodo de herramientas y una arista condicional: el ciclo entero del agente como un grafo de LangGraph.
7. [Callbacks y aprobación humana](07-callbacks-and-approval.md): Observar una ejecución con callbacks, y pausarla antes de una herramienta riesgosa para que una persona la apruebe o la rechace.
8. [Guardrails](08-guardrails.md): Validaciones en código alrededor del modelo: validaciones de entrada y salida, listas de herramientas permitidas y límites de pasos, en capas.

https://agenticademy.org/es/paths/first-agents
