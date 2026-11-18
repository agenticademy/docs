# Bucles de planificación

Agentes avanzados · Capítulo 7 de 10 · 20 minutos

Plan-and-execute, ReWOO y LLMCompiler planifican el trabajo antes de hacerlo, y difieren en llamadas al modelo, supersteps y tiempo.

## Objetivos

- Ejecuta plan-and-execute: un plan, un paso a la vez y una replanificación después de cada paso.
- Ejecuta ReWOO: un plan con marcadores #E, las herramientas y luego una respuesta.
- Ejecuta LLMCompiler: un grafo de tareas cuyas tareas independientes se ejecutan al mismo tiempo.
- Supera la comprobación.

## La lectura

### Primero planificar, después actuar

Los agentes vistos hasta ahora decidían una llamada a herramienta a la vez: llamar al modelo, ejecutar la herramienta, llamar otra vez al modelo. Ese es el bucle ReAct. Un agente que planifica primero le pide un plan al modelo y luego lo recorre. Tres patrones publicados lo hacen de maneras distintas. El laboratorio los compara con tres contadores: llamadas al modelo, supersteps (una ronda de ejecuciones de nodos, como en el capítulo 1) y cuánto tarda una ejecución.

### Plan-and-execute

Un nodo planner escribe una lista de pasos. Un nodo agent hace el primero: el modelo elige una llamada a herramienta y el nodo la ejecuta. Un nodo replan lee lo que está hecho y luego responde o reescribe los pasos que quedan. El plan puede cambiar después de cada resultado, pero cada paso cuesta al menos dos llamadas al modelo: una para actuar y otra para replanificar. La idea viene del artículo Plan-and-Solve (2023) y del tutorial plan-and-execute de LangGraph.

### ReWOO

ReWOO (2023) escribe todo el plan en una sola llamada al modelo y nombra cada resultado antes de que exista: #E1 = search[…], luego #E3 = forecast[#E1]. #E1 es un marcador de posición. Un nodo tool ejecuta los pasos en orden y pone cada resultado en lugar de su nombre, sin llamar al modelo. Un nodo solve llama al modelo una vez más para responder. Son dos llamadas al modelo en total, pero el plan no puede cambiar una vez escrito.

### LLMCompiler

LLMCompiler (2023) escribe el plan como un grafo de tareas sin ciclos, un DAG: 3. forecast($1) significa que la tarea 3 necesita el resultado de la tarea 1. Un scheduler, la unidad de obtención de tareas (task fetching unit) del artículo, inicia cada tarea en cuanto terminan las tareas que necesita, así que las tareas que no necesitan nada unas de otras se ejecutan al mismo tiempo. Un nodo join lee los resultados y luego responde o pide un plan nuevo.

### En este laboratorio

Una tarea de investigación se ejecuta de tres maneras: el pronóstico del sábado donde empieza cada una de dos carreras de trail, para Halden, la tienda ficticia. Cada grafo es LangGraph.js 1.5.1 real, dibujado a partir de getGraphAsync(). En Node también podrías escribir el scheduler con la API funcional, como un entrypoint cuyas llamadas a task() se ejecutan en paralelo. task() necesita AsyncLocalStorage, que la versión para navegador no tiene, así que aquí el nodo schedule inicia promesas simples y pasa config a mano. El modelo sigue un guion, y cada llamada tarda un tiempo fijado por el guion, acelerado en pantalla y contado como tiempo simulado.

## Pruébalo: Una tarea, tres planificadores

Ejecuta la misma tarea de investigación como plan-and-execute, ReWOO y LLMCompiler, y luego compara sus llamadas al modelo, supersteps y tiempo simulado.

https://agenticademy.org/es/paths/advanced-agents/07-planning-loops

## Comprobación

### Pregunta 1 de 3

Una tarea necesita cinco búsquedas que no dependen entre sí y luego una respuesta. ¿Qué patrón puede terminar antes?

- Plan-and-execute: replan mantiene el plan al día.
- ReWOO: llama al modelo solo dos veces.
- **✓** LLMCompiler: las cinco búsquedas pueden ejecutarse al mismo tiempo.
- Los tres tardan lo mismo, porque las búsquedas son las mismas.

**Por qué:** El scheduler de LLMCompiler inicia cada tarea cuyas entradas están listas, así que las cinco búsquedas se superponen y tardan más o menos lo que la más lenta. ReWOO también llama al modelo solo dos veces, pero su nodo tool ejecuta una búsqueda por superstep, una tras otra. Plan-and-execute agrega dos llamadas al modelo por búsqueda. Las búsquedas son las mismas; el momento en que se ejecutan, no.

### Pregunta 2 de 3

Un plan de ReWOO tiene el paso #E3 = forecast[#E1]. ¿Qué pasa con #E1 antes de que se ejecute forecast?

- **✓** El nodo tool pone el resultado del paso #E1 en su lugar.
- Se llama otra vez al modelo para convertir #E1 en un pueblo.
- forecast recibe el texto “#E1” y lo busca.
- El nodo solve lo completa al final.

**Por qué:** #E1 es un marcador de posición para el resultado del paso llamado #E1. El nodo tool lo reemplaza con ese resultado antes de llamar a forecast, así que no hace falta ninguna llamada al modelo entre pasos. Así ReWOO se arregla con dos llamadas al modelo: una para planificar y otra en el nodo solve, que se ejecuta después de todas las herramientas, demasiado tarde para completar nada.

### Pregunta 3 de 3

La Lake Loop se cancela, así que la búsqueda de su pueblo de salida devuelve “Sin resultado.” ¿Qué patrón puede quitar el pronóstico de la Lake Loop antes de que se ejecute?

- **✓** Plan-and-execute: replan lee cada resultado antes del siguiente paso.
- ReWOO: el nodo tool omite un paso cuya entrada no encontró nada.
- LLMCompiler: el scheduler omite una tarea cuya entrada no encontró nada.
- Ninguno: un plan no puede cambiar una vez escrito.

**Por qué:** Plan-and-execute llama a replan después de cada paso, así que puede quitar el pronóstico de los pasos pendientes: eso es lo que pagan sus llamadas extra al modelo. El nodo tool de ReWOO solo completa resultados, así que ejecuta forecast[#E2] con “Sin resultado.” como pueblo. El scheduler de LLMCompiler también ejecuta forecast($2) según el plan; solo el nodo join, después de todas las tareas, puede pedir un plan nuevo.

## Lo que ya puedes hacer

- Construir plan-and-execute, ReWOO y LLMCompiler como grafos de LangGraph.js.
- Contar lo que cuesta cada patrón en llamadas al modelo, supersteps y tiempo, y decir de dónde viene la diferencia.
- Elegir un patrón: replanificar cuando los resultados pueden cambiar el plan, planificar una vez cuando no pueden, y ejecutar al mismo tiempo los pasos independientes.

---

[Volver a Agentes avanzados](README.md) · [Siguiente: Gastar cómputo](08-spending-compute.md)
