# Agentes avanzados

Avanzado · Para desarrolladores · 10 capítulos · 205 min

Para desarrolladores que ya saben construir sistemas de agentes. Trabajas con grafos grandes y su estado, enrutas con Command y Send, rebobinas y bifurcas ejecuciones, construyes ciclos que se recuperan de los fallos y saben cuándo parar, comparas patrones de planificación y ejecutas un harness de deep agents en un trabajo largo. Cada laboratorio ejecuta LangGraph.js 1.5.1 real en tu navegador, con un modelo que sigue un guion. El harness de deep agents es recreado, y las pantallas lo dicen.

## Antes de empezar

La ruta Sistemas de agentes y estándares, o el mismo conocimiento: grafos, herramientas, el ciclo del agente, checkpoints y trazas.

## Al final podrás

- Diseñar el estado de un grafo con reducers y Overwrite, y predecir cómo se combinan las escrituras en paralelo.
- Enrutar con Command y Send, y formar equipos con subgrafos.
- Rebobinar, editar y bifurcar una ejecución desde sus checkpoints.
- Construir ciclos que reintentan, compensan, se reanudan tras una caída y se detienen por límites, presupuestos o la decisión de una persona.
- Elegir un patrón de planificación o de gasto de cómputo, y ejecutar un harness de deep agents en un trabajo largo.
- Llevar un trabajo largo a lo largo de varias sesiones con archivos de traspaso, compactación y un Store, y evaluar su trayectoria con reglas en código.

## Capítulos

1. [El estado a escala](01-state-at-scale.md): Cómo LangGraph.js 1.5.1 combina las escrituras en una clave del estado: campos de StateSchema con y sin reducer, el orden de las escrituras en paralelo, Overwrite y UntrackedValue.
2. [Command y Send](02-command-and-send.md): Un nodo puede actualizar el estado y elegir el siguiente nodo en un solo paso con Command, y repartir el trabajo entre workers en paralelo con Send, tantos como pida el modelo.
3. [Subgrafos y equipos](03-subgraphs-and-teams.md): Arma equipos de agentes con subgrafos, decide cómo comparten el estado, lee sus namespaces en el stream y compara patrones multiagente por llamadas al modelo y tokens.
4. [Viajar en el tiempo](04-time-travel.md): Cada superstep guarda un checkpoint, así que puedes leer la historia de una ejecución, repetirla desde un paso pasado, editar un estado pasado y bifurcar un segundo final.
5. [Bucles que sobreviven](05-loops-that-survive.md): Las políticas de reintento, los timeouts, los manejadores de errores y los checkpoints mantienen en marcha una ejecución larga cuando una herramienta falla o el proceso se detiene.
6. [Bucles que se detienen](06-loops-that-stop.md): Un bucle necesita paradas que se cumplan haga lo que haga el modelo: un límite de recursión, un presupuesto de llamadas, un detector de llamadas repetidas y una persona que puede decir que no.
7. [Bucles de planificación](07-planning-loops.md): Plan-and-execute, ReWOO y LLMCompiler planifican el trabajo antes de hacerlo, y difieren en llamadas al modelo, supersteps y tiempo.
8. [Gastar cómputo](08-spending-compute.md): Los bucles evaluador-optimizador, la reflexión, best-of-n y la búsqueda en árbol gastan más llamadas al modelo para lograr una mejor respuesta, y cada uno necesita un presupuesto.
9. [Un harness de deep agent](09-deep-agent-harness.md): Un deep agent planifica con una lista de pendientes, guarda notas en archivos virtuales, pasa partes de un trabajo a subagentes con un contexto nuevo y se detiene antes de las herramientas que nombres.
10. [Agentes de largo horizonte](10-long-horizon.md): Un trabajo que abarca varias sesiones necesita compactación dentro de una sesión, archivos de traspaso y memoria entre sesiones, y una revisión de toda su trayectoria.

https://agenticademy.org/es/paths/advanced-agents
