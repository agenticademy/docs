# El ciclo del agente: un agente ReAct mínimo

Construye tus primeros agentes · Capítulo 6 de 8 · 20 minutos

Un nodo de modelo, un nodo de herramientas y una arista condicional: el ciclo entero del agente como un grafo de LangGraph.

## Objetivos

- Avanza el agente un superstep a la vez hasta que responda.
- Cuenta cuántas veces se ejecutó el modelo.
- Ordena la ejecución.

## La lectura

### Razonar, actuar, observar, repetir

ReAct (razonar y actuar) es un patrón para agentes: el modelo lee la conversación y o bien pide herramientas o bien responde. Cuando pide, tu código ejecuta las herramientas y agrega sus resultados a la conversación, y el modelo vuelve a leer. El ciclo termina cuando el modelo responde sin pedir ninguna herramienta.

### El ciclo como grafo

En LangGraph el ciclo son dos nodos sobre MessagesAnnotation, el estado que contiene la lista de mensajes. El nodo agent llama al modelo, con las herramientas vinculadas, sobre state.messages y devuelve su respuesta. El nodo tools es ToolNode de @langchain/langgraph/prebuilt: ejecuta cada llamada a herramienta del último AIMessage y agrega un ToolMessage por llamada. Los argumentos inválidos se convierten en ToolMessages de error, como en el capítulo 5.

toolsCondition es el router después del nodo agent: devuelve "tools" cuando el último mensaje tiene tool\_calls y END cuando no tiene. Una arista lleva de tools de vuelta a agent. Ese es el agente entero, unas 25 líneas.

LangGraph ejecuta un grafo en supersteps: en cada uno, los nodos que tocan se ejecutan una vez. En este grafo solo toca un nodo a la vez, así que cada superstep es una ejecución de un nodo, y graph.stream(input, { streamMode: "updates" }) entrega una actualización por superstep. Puedes ver girar el ciclo.

### Listo para usar, y con límite

createReactAgent de @langchain/langgraph/prebuilt construye el mismo grafo; está obsoleto en favor de createAgent del paquete langchain. Constrúyelo a mano una vez y sabrás qué hacen esos atajos.

Un ciclo puede seguir para siempre si el modelo sigue pidiendo herramientas. recursionLimit en la config de la ejecución limita los supersteps (25 por defecto); cuando se alcanza, LangGraph lanza GraphRecursionError. Prueba abajo un límite de 4: esta ejecución necesita 5 supersteps.

## Pruébalo: El ciclo, un paso a la vez

Tres pasos, una acción cada uno, y un cuarto opcional: envía el ticket T-48213, avanza el ciclo hasta que el agente responda, cuenta las llamadas al modelo y luego limita el ciclo con recursionLimit.

https://agenticademy.org/es/paths/first-agents/06-the-agent-loop

## Comprobación

Ordena la ejecución del ticket T-48213.

1. El nodo agent envía la tarea al modelo.
2. El modelo pide get\_ticket.
3. ToolNode ejecuta get\_ticket y agrega un ToolMessage.
4. El modelo lo lee y pide list\_charges.
5. ToolNode ejecuta list\_charges y agrega un ToolMessage.
6. El modelo responde sin llamadas a herramientas.
7. toolsCondition enruta a END.

**Por qué:** Cada vuelta del modelo o bien pide herramientas, lo que da otra vuelta por ToolNode, o bien responde, lo que termina la ejecución. El modelo se ejecutó tres veces.

## Lo que ya puedes hacer

- Construir un agente ReAct como grafo con ToolNode y toolsCondition.
- Seguir una ejecución mensaje por mensaje con streamMode "updates".
- Explicar por qué todo ciclo de agente necesita un límite de recursión.

---

[Volver a Construye tus primeros agentes](README.md) · [Siguiente: Callbacks y aprobación humana](07-callbacks-and-approval.md)
