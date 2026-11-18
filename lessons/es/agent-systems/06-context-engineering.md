# Ingeniería de contexto y recuperación

Sistemas de agentes y estándares · Capítulo 6 de 8 · 25 minutos

Elegir lo que lee cada llamada al modelo: grep o búsqueda vectorial, descargar resultados grandes, un primer vistazo a los subagentes y una memoria que dura entre hilos.

## Objetivos

- Ejecuta grep y búsqueda vectorial con las mismas palabras y compara.
- Deja que un resultado de herramienta grande se descargue en lugar de llenar el contexto.
- Pásales una búsqueda a subagentes y compara los tamaños de contexto.
- Supera la comprobación.

## La lectura

### Lo que lee cada llamada

Una llamada al modelo lee su contexto: el prompt de sistema, los mensajes hasta ahora y cada resultado de herramienta. La ingeniería de contexto es decidir qué entra. Muy poco y el agente adivina; demasiado y cuesta más, va más lento y se distrae con texto que no importa, o que alguien plantó.

### grep o vectores

grep encuentra texto exacto, línea por línea: un número de pedido, un id de ticket, una fecha. No cuesta nada y se pierde todo lo que esté redactado de otra forma.

La búsqueda vectorial encuentra significados parecidos. Cada fragmento de cada archivo se convierte en un vector (aquí, 384 números del modelo de embeddings all-MiniLM-L6-v2), y los fragmentos se ordenan por similitud coseno con el vector de la consulta. Encuentra “payment taken two times” para “charged twice”, pero para 48213 pone los tickets de los pedidos 48144 y 48231 por delante del correcto. Filtros como una carpeta o una fecha dejan afuera fragmentos viejos o no relacionados.

### Descargar, skills y subagentes

Una herramienta puede devolver mucho más de lo que cabe en un contexto. El desalojo guarda un resultado grande en un archivo y le da al modelo la ruta y una vista previa corta; luego el agente hace grep sobre el archivo y lee solo las líneas que necesita.

Las skills siguen la misma idea: el agente ve el nombre de cada skill y una descripción de una línea, y lee sus instrucciones completas (un archivo SKILL.md) solo cuando una tarea las necesita.

Un subagente recibe un contexto nuevo con solo su tarea y devuelve un informe, así que lo que lee queda fuera del contexto principal. Agentes avanzados, capítulos 9 y 10, trata a fondo los subagentes, los umbrales y la compactación.

### Memoria entre hilos

Un hilo es una conversación: su checkpointer guarda los mensajes de ese hilo, y un hilo nuevo empieza vacío. La memoria de largo plazo es un store que comparten todos los hilos. El InMemoryStore de LangGraph guarda elementos bajo un namespace, como customers / mara-lind; un hilo deja una nota ahí y un hilo posterior la busca. En producción, el store es una base de datos.

### En este laboratorio

El agente de soporte de Halden trabaja el ticket T-48213. Su harness recrea Deep Agents (un harness de código abierto de LangChain) sobre LangGraph.js: herramientas de sistema de archivos, desalojo por encima de 20 000 tokens y subagentes de tareas. El paso de memoria usa el InMemoryStore propio de LangGraph.js.

## Pruébalo: Lo que lee el agente

Busca las mismas palabras con grep y con vectores, deja que se descargue la exportación de pagos, pásales la lectura a subagentes y luego guarda una nota que un hilo posterior vuelve a leer.

https://agenticademy.org/es/paths/agent-systems/06-context-engineering

## Comprobación

### Pregunta 1 de 4

El agente debe encontrar cada ticket que menciona el pedido 48213. ¿Qué búsqueda encaja mejor?

- Búsqueda vectorial.
- **✓** grep de 48213.
- Pedirle al modelo que lo recuerde.

**Por qué:** Un id exacto es justo para lo que sirve grep. La búsqueda vectorial ordena por significado, y en este laboratorio pone los tickets de los pedidos 48144 y 48231 por delante del correcto.

### Pregunta 2 de 4

Una herramienta devuelve una exportación de 40 000 filas. ¿Qué pone el desalojo en el contexto?

- Las 40 000 filas.
- **✓** La ruta del archivo y una vista previa corta; el agente lee páginas de él cuando las necesita.
- Nada en absoluto.

**Por qué:** El resultado completo se guarda en un archivo, así que el contexto se mantiene pequeño y el agente igual puede hacer grep y paginar los datos.

### Pregunta 3 de 4

¿Qué vuelve de un subagente al agente que lo inició?

- Su conversación entera.
- **✓** Un informe final de su trabajo.
- Sus herramientas.

**Por qué:** La lectura del subagente se queda en su propio contexto. Solo su informe entra al contexto del padre, lo que mantiene pequeño al padre y deja afuera el texto plantado.

### Pregunta 4 de 4

El hilo 1 guardó una nota en el store. El hilo 2 empieza sin mensajes. ¿Puede leer la nota?

- No: un hilo nuevo empieza vacío.
- **✓** Sí: todos los hilos comparten el store; solo los mensajes pertenecen a un hilo.
- Solo si el hilo 2 copia los mensajes del hilo 1.

**Por qué:** El checkpointer mantiene separados los mensajes de cada hilo. El store está fuera de los hilos, así que cualquier hilo puede buscar en el namespace de la clienta.

## Lo que ya puedes hacer

- Elegir entre grep y búsqueda vectorial para una consulta, y decir cuándo ayuda un filtro por carpeta o por fecha.
- Descargar resultados grandes de herramientas y paginarlos.
- Usar subagentes para mantener pequeño y limpio el contexto de un agente principal.
- Guardar una nota en un store de largo plazo y volver a leerla en un hilo posterior.

---

[Volver a Sistemas de agentes y estándares](README.md) · [Siguiente: Observabilidad: trazas y ejecuciones](07-observability.md)
