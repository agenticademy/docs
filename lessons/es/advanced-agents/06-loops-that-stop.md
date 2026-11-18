# Bucles que se detienen

Agentes avanzados · Capítulo 6 de 10 · 20 minutos

Un bucle necesita paradas que se cumplan haga lo que haga el modelo: un límite de recursión, un presupuesto de llamadas, un detector de llamadas repetidas y una persona que puede decir que no.

## Objetivos

- Ejecuta un agente que repite una llamada a una herramienta hasta que GraphRecursionError lo detiene.
- Activa un presupuesto de llamadas y un detector de repeticiones, y mira cuál se dispara primero.
- Pausa antes de una herramienta riesgosa y rechaza la llamada.
- Supera la comprobación.

## La lectura

### Un bucle necesita paradas que el modelo no elige

Un bucle de agente termina cuando el modelo responde sin pedir una herramienta. Un turno es una llamada al modelo más las herramientas que pide. Si el modelo sigue pidiendo, por ejemplo para revisar un estado que nunca cambia, cada turno es otra llamada pagada al modelo y no se aprende nada nuevo. Por eso agregas paradas que se cumplen haga lo que haga el modelo.

### El tope de LangGraph: recursionLimit

En un bucle como este, cada ejecución de un nodo es un superstep. recursionLimit, en la config de la ejecución, pone un tope a los supersteps, 25 por defecto. Al pasarlo, LangGraph.js lanza GraphRecursionError, con lc\_error\_code GRAPH\_RECURSION\_LIMIT. Es un último recurso: cuenta supersteps, no turnos, se detiene tarde y, sin un checkpointer, el estado de la ejecución se pierde con el error.

### Paradas como middleware

El middleware son hooks que el createAgent de LangChain 1.6 ejecuta alrededor de la llamada al modelo; los hooks beforeModel y afterModel se convierten cada uno en un nodo del grafo. modelCallLimitMiddleware es un presupuesto en llamadas al modelo: runLimit cuenta una ejecución, threadLimit un hilo entero, y exitBehavior 'end' termina la ejecución con un mensaje. toolCallLimitMiddleware cuenta las llamadas a una herramienta o a todas. Un presupuesto en tokens o en dinero funciona igual: contar, comparar, detener. Este sitio recrea los dos, como modelCallLimit y toolCallLimit; el laboratorio usa modelCallLimit.

Un detector de repeticiones atrapa pronto un bucle atascado: la misma herramienta con los mismos argumentos, threshold veces en las últimas llamadas. El repeatDetector de aquí es propio de este sitio; LangChain no trae uno integrado. Con exitBehavior 'end' termina la ejecución; con 'continue' rechaza la llamada y le dice al modelo que pruebe otra cosa. Aquí cada hook también escribe un evento de traza, así que puedes ver qué parada se disparó.

### Una persona como parada

Algunas llamadas deberían esperar un sí, como enviar dinero. interruptBefore pausa el grafo antes de un nodo, y un checkpointer guarda el estado en pausa. El punto de interrupción se detiene antes de cada ejecución de ese nodo, así que tu código elige las llamadas que necesitan a una persona y se las muestra. Para rechazar, updateState responde la llamada con un ToolMessage como si el nodo tools se hubiera ejecutado; invocar otra vez con una entrada null reanuda la ejecución. En Node, humanInTheLoopMiddleware hace esto con interrupt(), que necesita AsyncLocalStorage; los navegadores no lo tienen, así que esta página usa el punto de interrupción.

## Pruébalo: Tres formas de detener un bucle

Un agente con guion revisa un reembolso que sigue pendiente. Detenlo con recursionLimit, después con un presupuesto y un detector de repeticiones, y después con una persona antes de la herramienta riesgosa.

https://agenticademy.org/es/paths/advanced-agents/06-loops-that-stop

## Comprobación

El modelo pide check\_refund({ order: '4471' }) en cada turno y nunca responde. ¿Qué parada termina esta ejecución?

Código:

```
const agent = createAgent({
  model,
  tools: [checkRefund],
  middleware: [
    modelCallLimit({
      runLimit: 3,
      exitBehavior: 'end',
    }),
    repeatDetector({
      window: 6,
      threshold: 4,
      exitBehavior: 'end',
    }),
  ],
});
// recursionLimit: 25 (the default)
await agent.graph.invoke({
  messages: [task],
});
```

- GraphRecursionError, en el superstep 25.
- El detector de repeticiones, en la cuarta llamada idéntica.
- **✓** El presupuesto de llamadas, antes de la cuarta llamada al modelo.
- Ninguna: una ejecución solo termina cuando el modelo responde.

**Por qué:** El presupuesto de llamadas permite tres llamadas al modelo; antes de la cuarta, modelCallLimit termina la ejecución. Así que el detector de repeticiones nunca ve una cuarta llamada idéntica. Aquí cada turno toma cinco supersteps, así que la ejecución termina en el superstep 16, mucho antes de los 25 de recursionLimit. Y una parada nunca espera al modelo: para eso es una parada.

## Lo que ya puedes hacer

- Configurar recursionLimit como último recurso y atrapar GraphRecursionError.
- Agregar un presupuesto de llamadas y un detector de repeticiones como middleware, y leer en su evento de traza cuál se disparó.
- Pausar antes de una herramienta riesgosa con interruptBefore, y rechazar la llamada con updateState.

---

[Volver a Agentes avanzados](README.md) · [Siguiente: Bucles de planificación](07-planning-loops.md)
