# Callbacks y aprobación humana

Construye tus primeros agentes · Capítulo 7 de 8 · 22 minutos

Observar una ejecución con callbacks, y pausarla antes de una herramienta riesgosa para que una persona la apruebe o la rechace.

## Objetivos

- Agrega un callback handler y mira sus eventos.
- Pausa antes de la herramienta de reembolso y luego apruébala.
- Rechaza un reembolso y mira cómo lo maneja el agente.
- Supera la comprobación.

## La lectura

### Los callbacks observan

Un callback handler es un objeto cuyos métodos LangChain llama a medida que avanza una ejecución: una subclase de BaseCallbackHandler con métodos como handleChatModelStart, handleLLMEnd, handleToolStart y handleToolEnd. Lo pasas en config.callbacks, y llega a cada llamada anidada que recibe esa config. Úsalo para logs, métricas y trazas. Un callback solo observa: LangChain ignora lo que devuelven sus métodos y, por defecto, solo registra un error lanzado dentro de uno, así que no puede cambiar ni detener un paso.

### Los breakpoints pausan

Para detener una ejecución antes de un paso, LangGraph necesita guardar el estado y volver a él. Eso es un checkpointer: compile({ checkpointer: new MemorySaver() }) guarda un checkpoint después de cada superstep, bajo el thread\_id de config.configurable. Agrega interruptBefore: ["approval\_tools"] y la ejecución se detiene antes de ese nodo.

Mientras está pausada, (await graph.getState(config)).next dice qué está esperando, y el último AIMessage contiene la llamada pendiente. Para aprobar, reanuda con graph.invoke(null, config): una entrada null significa “sigue desde el checkpoint”. Para rechazar, primero llama a graph.updateState(config, { messages: [toolMessage] }, "approval\_tools"): tu ToolMessage responde la llamada como si el nodo se hubiera ejecutado, así que el nodo se salta y el modelo lee tu rechazo.

Dale a las herramientas riesgosas su propio nodo, como aquí con create\_refund, para que la puerta detenga solo a ellas.

### En Node, y en otros lugares

En Node, createAgent del paquete langchain ofrece hooks de middleware (beforeModel, afterModel, wrapModelCall, wrapToolCall) y humanInTheLoopMiddleware, que pausa con interrupt(). interrupt() necesita AsyncLocalStorage, así que en un navegador usas breakpoints, como aquí. Los agentes de programación usan la misma idea: hooks que se ejecutan antes de una llamada a herramienta y pueden denegarla.

## Pruébalo: Una puerta antes del reembolso

Cuatro pasos, una acción cada uno, y un quinto opcional: mira los eventos de callback de una ejecución, pon una puerta antes del reembolso, aprueba una ejecución y luego rechaza la siguiente.

https://agenticademy.org/es/paths/first-agents/07-callbacks-and-approval

## Comprobación

### Pregunta 1 de 3

¿Qué necesita interruptBefore para funcionar?

- **✓** Un checkpointer, y un thread\_id en la config.
- Un callback handler.
- Un modelo real.

**Por qué:** Pausar significa guardar el estado y volver a él. El checkpointer lo guarda bajo el id del thread.

### Pregunta 2 de 3

La ejecución está pausada antes de approval\_tools. ¿Cómo la continúas tal como estaba?

- Invocar de nuevo con la entrada original.
- **✓** graph.invoke(null, config) con el mismo thread\_id.
- Empezar un thread nuevo.

**Por qué:** Una entrada null le dice a LangGraph que reanude desde el checkpoint guardado en lugar de empezar una ejecución nueva.

### Pregunta 3 de 3

El handleToolStart de un callback handler devuelve false cuando ve create\_refund. ¿Qué pasa?

- El reembolso queda bloqueado.
- **✓** Nada cambia: LangChain ignora el valor devuelto, así que el reembolso se ejecuta. Para detener un paso, pausa el grafo o protege la herramienta.
- La ejecución se pausa antes de approval\_tools.

**Por qué:** Los callbacks son para observar y registrar. El control va en el grafo: un breakpoint con updateState, como aquí, o una validación dentro del nodo de herramientas.

## Lo que ya puedes hacer

- Registrar los eventos de modelo y herramientas de una ejecución con un callback handler.
- Pausar un grafo antes de un nodo con un checkpointer e interruptBefore.
- Reanudar o rechazar un paso pendiente con getState, updateState e invoke(null).

---

[Volver a Construye tus primeros agentes](README.md) · [Siguiente: Guardrails](08-guardrails.md)
