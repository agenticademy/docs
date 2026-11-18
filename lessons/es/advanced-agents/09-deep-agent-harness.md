# Un harness de deep agent

Agentes avanzados · Capítulo 9 de 10 · 20 minutos

Un deep agent planifica con una lista de pendientes, guarda notas en archivos virtuales, pasa partes de un trabajo a subagentes con un contexto nuevo y se detiene antes de las herramientas que nombres.

## Objetivos

- Inicia el trabajo y mira cómo el agente escribe y actualiza su lista de pendientes.
- Aprueba la escritura de las notas y mira cómo crece el árbol de archivos.
- Ejecuta tres subagentes en paralelo y compara sus tamaños de contexto.
- Aprueba la verificación.

## La lectura

### Un harness para trabajos largos

Un harness es el código alrededor del bucle del modelo: el prompt, las herramientas y lo que ve cada llamada. El paquete de código abierto deepagents de LangChain es uno, construido sobre LangGraph. Su anuncio de 2025 nombró cuatro partes: un prompt detallado, una herramienta de planificación, subagentes y un sistema de archivos.

### Pendientes y archivos

write\_todos({ todos }) recibe la lista completa cada vez, cada elemento pending, in\_progress o completed, y la guarda en state.todos. No hace nada más: la ventaja es que el modelo vuelve a leer su plan en cada llamada. Dos llamadas en un mismo turno se rechazan. Se activa al agregar todoListMiddleware() de LangChain a middleware.

Las herramientas de archivos (ls, read\_file, write\_file, edit\_file, glob, grep) trabajan sobre state.files, un sistema de archivos virtual en el estado del grafo. Los subagentes pueden leer las notas que hay ahí, y estas sobreviven a un resumen de los mensajes antiguos.

### Subagentes

task({ description, subagent\_type }) inicia un subagente con un contexto nuevo: su propio system prompt y la descripción, ninguno de los mensajes de quien lo llama. Empieza con los archivos de quien lo llama, los archivos que escribe se fusionan de vuelta y solo su último mensaje regresa, como resultado de la herramienta. Las llamadas de un mismo turno se ejecutan en paralelo. Por defecto hay un subagente de uso general con las herramientas de quien lo llama.

### Skills, memoria y aprobaciones

Un skill es una carpeta con un archivo SKILL.md. Con skills: ['/skills/'], el prompt lista el nombre y la descripción de cada skill, y el agente lee el archivo solo cuando una tarea lo necesita. La opción memory carga archivos AGENTS.md en el prompt. interruptOn, con un checkpointer, nombra herramientas que pausan la ejecución hasta que una persona aprueba, edita o rechaza cada llamada. Esta página se pausa en un punto de interrupción (interruptBefore), porque interrupt() necesita AsyncLocalStorage.

### Menos andamiaje

Cada parte cuesta tokens en cada llamada. deepagents 1.12 (2026-07-29) hizo opcional la lista de pendientes y dejó vacío su system prompt genérico: los modelos actuales necesitan menos guía. La publicación de Anthropic sobre el diseño de harness (2026-03-24) coincide: con cada modelo nuevo, quita las partes que ya no necesita.

### En este laboratorio

Un harness recreado escribe un informe sobre las quejas por reembolsos en Halden, una tienda ficticia. A diferencia de deepagents 1.16, activa la lista con todos: true, muestra los skills al investigador sin una lista de skills propia, conserva la antigua línea genérica del prompt y no tiene memoria AGENTS.md. El agente principal se pausa antes de write\_file y task para que puedas seguirlo; en tu propio agente, pausa antes de las herramientas que cambian cosas fuera de él.

## Pruébalo: Un trabajo de investigación, una parte a la vez

Inicia el trabajo y mira la lista de pendientes, deja que el agente principal escriba sus notas, luego aprueba tres subagentes y compara sus tamaños de contexto.

https://agenticademy.org/es/paths/advanced-agents/09-deep-agent-harness

## Comprobación

El contexto del agente principal tiene unos 1400 tokens cuando su modelo hace esta llamada. El investigador lee unos 600 tokens de archivos, edita la nota y termina con un informe de 40 tokens. ¿Qué cambia en el estado del agente principal?

Código:

```
const lead = createDeepAgent({
  systemPrompt: 'You write briefs.',
  subagents: [{
    name: 'researcher',
    description: 'Researches a cause.',
    systemPrompt: 'Edit the note, then report.',
  }],
});

// The lead's model asks for:
task({
  subagent_type: 'researcher',
  description:
    'Late refunds: /notes/late-refund.md',
})
```

- Sus mensajes reciben toda la conversación del investigador, unos 650 tokens; sus archivos no cambian.
- Sus mensajes reciben un resultado de herramienta, el informe de 40 tokens; sus archivos no cambian, porque el investigador editó una copia.
- **✓** Sus mensajes reciben un resultado de herramienta, el informe de 40 tokens, y la nota editada se fusiona con sus archivos.
- Nada cambia hasta que el agente principal lea /notes/late-refund.md por su cuenta.

**Por qué:** task inicia al investigador con un contexto nuevo: su propio system prompt y la descripción, ninguno de los mensajes del agente principal. Lo que lee queda en su propio contexto. Solo su último mensaje vuelve, como resultado de la herramienta task, así que el agente principal crece unos 40 tokens, y los archivos que escribió se fusionan con el state.files del agente principal. En el laboratorio, tres investigadores leyeron miles de tokens mientras el agente principal creció en tres informes cortos.

## Lo que ya puedes hacer

- Activar la lista de pendientes de un deep agent agregando todoListMiddleware() a su middleware, y leer el plan en state.todos.
- Guardar notas y borradores en state.files con ls, read\_file, write\_file, edit\_file, glob y grep.
- Pasar trabajo autónomo a subagentes con task, y usar los informes y archivos que vuelven.
- Pausar un deep agent antes de las herramientas elegidas con interruptOn y un checkpointer, y quitar el andamiaje que un modelo más nuevo ya no necesita.

---

[Volver a Agentes avanzados](README.md) · [Siguiente: Agentes de largo horizonte](10-long-horizon.md)
