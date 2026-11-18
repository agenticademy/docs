# Sistemas de agentes y estándares

Intermedio · Para desarrolladores · 8 capítulos · 195 min

Para desarrolladores que ya saben construir un solo agente. Conectas agentes con herramientas, con otros agentes, con interfaces y con pagos usando los estándares abiertos, orquestas varios agentes en un grafo, decides qué entra en cada contexto, trazas ejecuciones y las pruebas contra ataques. Las librerías de los protocolos se ejecutan de verdad en tu navegador; puedes retener, descartar y alterar cada mensaje.

## Antes de empezar

La ruta Construye tus primeros agentes, o el mismo conocimiento: mensajes y herramientas de LangChain.js, grafos de LangGraph y el ciclo del agente.

## Al final podrás

- Conectar un agente a un servidor MCP con el protocolo 2026-07-28, incluidas las peticiones de input a una persona.
- Delegar trabajo a otro agente con tareas A2A y Agent Cards.
- Transmitir el trabajo de un agente a una interfaz con AG-UI, y describir una UI segura con A2UI.
- Explicar cómo UCP y AP2 atan la compra de un agente al consentimiento firmado de una persona.
- Orquestar agentes con supervisores, fan-out con Send y subgrafos.
- Diseñar el contexto con búsqueda, desalojo y subagentes, y trazar ejecuciones en el formato de LangSmith.
- Probar un sistema de agentes contra la inyección de prompts y la alteración de mensajes, usando los riesgos agénticos de OWASP como mapa.
- Leer una línea de tiempo de supersteps y un árbol de ejecuciones.
- Añadir a un agente un límite de llamadas, un límite de herramientas o una protección como middleware.

## Capítulos

1. [MCP: herramientas desde un servidor](01-mcp.md): Cómo un agente encuentra y llama herramientas en un servidor MCP, y cómo el protocolo 2026-07-28 le pregunta a una persona antes de una llamada riesgosa.
2. [A2A: agentes que delegan en agentes](02-a2a.md): Cómo un agente encuentra a otro mediante su Agent Card y le pasa una tarea que puede seguir hasta el final.
3. [Agentes e interfaces: AG-UI y A2UI](03-agents-and-uis.md): Transmitir el trabajo de un agente a una app con AG-UI, y dejar que un agente describa componentes de interfaz seguros con A2UI.
4. [Agentes que pagan: UCP y AP2](04-agent-commerce.md): Cómo un agente completa una compra con un comercio por UCP, y cómo los mandatos de AP2 atan el pago a lo que firmó la persona.
5. [Orquestación: supervisores, fan-out, subgrafos](05-orchestration.md): Patrones para varios agentes en un solo grafo de LangGraph: un supervisor que enruta, Send para trabajo en paralelo, subgrafos y el checkpoint que guarda cada superstep.
6. [Ingeniería de contexto y recuperación](06-context-engineering.md): Elegir lo que lee cada llamada al modelo: grep o búsqueda vectorial, descargar resultados grandes, un primer vistazo a los subagentes y una memoria que dura entre hilos.
7. [Observabilidad: trazas y ejecuciones](07-observability.md): Registrar cada llamada al modelo, cada llamada a herramienta y cada paso como un árbol de ejecuciones, y leerlo para encontrar lo que va lento o falla.
8. [Evaluación y seguridad](08-evaluation-and-safety.md): Probar un sistema de agentes con casos y validaciones, atacarlo con inyección y alteración de mensajes, y relacionar las fallas con los riesgos agénticos de OWASP.

https://agenticademy.org/es/paths/agent-systems
