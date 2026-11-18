# Observabilidad: trazas y ejecuciones

Sistemas de agentes y estándares · Capítulo 7 de 8 · 20 minutos

Registrar cada llamada al modelo, cada llamada a herramienta y cada paso como un árbol de ejecuciones, y leerlo para encontrar lo que va lento o falla.

## Objetivos

- Encuentra la ejecución de modelo o de herramienta más lenta de la traza.
- Encuentra la ejecución de herramienta que falló y su padre.
- Exporta las ejecuciones en el formato de LangSmith.
- Supera la comprobación.

## La lectura

### Ejecuciones y trazas

La respuesta de un agente esconde muchos pasos. El trazado registra cada uno como una ejecución (run): un grafo, un nodo, una llamada al modelo o una llamada a herramienta, con sus entradas, salidas, horas de inicio y fin, y cualquier error. Las ejecuciones detrás de una petición forman un árbol, llamado traza.

LangChain.js y LangGraph.js trazan mediante callbacks. Pasa un tracer como LangChainTracer en los callbacks de la config, y cada runnable le informa su inicio y su fin.

### El formato de ejecución de LangSmith

Cada ejecución tiene un run\_type, como chain, llm o tool, un id, un trace\_id compartido por todo el árbol y un parent\_run\_id. Su dotted\_order es una clave construida a partir de la hora de inicio y el id de cada ejecución desde la raíz hasta ella, así que ordenar por ella dispone el árbol en el orden en que pasaron las cosas. Las ejecuciones de modelo llevan usage\_metadata con conteos de tokens. LangGraph agrega thread\_id, langgraph\_node y langgraph\_step a la metadata.

### Supersteps y el árbol de ejecuciones

LangGraph ejecuta un grafo en supersteps. En cada superstep se ejecutan los nodos que tocan, y sus actualizaciones se aplican juntas antes de que empiece el siguiente superstep. langgraph\_step es el número de ese superstep, así que una traza se lee de dos maneras: como un árbol de ejecuciones, cada ejecución bajo su padre, y como una línea de tiempo de supersteps, un grupo de ejecuciones por paso. La cascada del laboratorio agrupa sus filas por superstep.

### Qué buscar

Latencia: qué ejecución es larga, y si es el modelo o una herramienta. Errores: una ejecución de herramienta que falló y el nodo que la llamó. Tokens: qué llamadas leen más. Ciclos: el mismo paso una y otra vez.

Las trazas contienen prompts, resultados de herramientas y datos de clientes, así que trátalas como sensibles. El SDK de LangSmith puede pasar un anonimizador por cada ejecución antes de enviarla. OpenTelemetry es el estándar general para trazas entre servicios; LangSmith también acepta trazas de OpenTelemetry.

## Pruébalo: La cascada de ejecuciones

Cinco pasos: ejecuta el agente y mira crecer su traza, lee sus supersteps, encuentra la ejecución más lenta y la herramienta que falló, y luego exporta las ejecuciones.

https://agenticademy.org/es/paths/agent-systems/07-observability

## Comprobación

### Pregunta 1 de 5

¿Qué te da dotted\_order?

- El costo de una ejecución.
- **✓** Una clave ordenable que ubica cada ejecución en el árbol de su traza, en orden temporal.
- El nombre del modelo.

**Por qué:** Se construye a partir de la hora de inicio y el id de cada ejecución desde la raíz hasta esta, así que ordenar las ejecuciones por ella reconstruye el árbol en el orden en que pasaron las cosas, sin seguir los vínculos al padre uno por uno.

### Pregunta 2 de 5

Dos ejecuciones de una traza tienen el mismo langgraph\_step. ¿Qué te dice eso?

- Las llamó la misma herramienta.
- **✓** Se ejecutaron en el mismo superstep del grafo.
- Tardaron lo mismo.

**Por qué:** langgraph\_step es el número del superstep. En cada superstep, LangGraph ejecuta los nodos que tocan y aplica sus actualizaciones juntas; cada ejecución dentro de esos nodos lleva ese número.

### Pregunta 3 de 5

¿Qué es una traza?

- Un archivo de log de errores.
- **✓** El árbol de ejecuciones, de modelo, herramienta y chain, detrás de una petición.
- Una captura de pantalla de la app.

**Por qué:** Una petición se convierte en una traza; cada paso dentro de ella es una ejecución con su padre.

### Pregunta 4 de 5

Una ejecución de herramienta falló, pero el agente igual respondió. ¿Por qué la ejecución padre no está marcada como fallida?

- El tracer oculta los errores.
- **✓** El nodo de herramientas atrapó el error y se lo devolvió al modelo como resultado, así que el nodo en sí terminó.
- Los padres nunca fallan.

**Por qué:** Un error se registra en la ejecución que lo lanzó. Aquí el nodo lo convirtió en un ToolMessage de error, el modelo lo leyó y reintentó.

### Pregunta 5 de 5

¿Qué deberías evitar enviar a un servicio de trazado compartido?

- Los tiempos de las ejecuciones.
- **✓** Datos personales de clientes dentro de prompts y resultados de herramientas, a menos que los necesites y tengas permiso.
- Los tipos de ejecución.

**Por qué:** Las trazas registran entradas y salidas completas. Oculta o limita lo que se captura antes de que salga de tu sistema, por ejemplo con el anonimizador del SDK.

## Lo que ya puedes hacer

- Leer una línea de tiempo de supersteps y un árbol de ejecuciones.
- Encontrar la ejecución lenta o fallida de una traza, y la ejecución que la llamó.
- Nombrar los campos principales del formato de ejecución de LangSmith.
- Ocultar datos personales antes de que una traza salga de tu sistema.

---

[Volver a Sistemas de agentes y estándares](README.md) · [Siguiente: Evaluación y seguridad](08-evaluation-and-safety.md)
