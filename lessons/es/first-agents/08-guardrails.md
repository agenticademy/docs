# Guardrails

Construye tus primeros agentes · Capítulo 8 de 8 · 22 minutos

Validaciones en código alrededor del modelo: validaciones de entrada y salida, listas de herramientas permitidas y límites de pasos, en capas.

## Objetivos

- Activa cada guardia y mira qué detiene.
- Detén los cuatro ataques en una sola repetición.
- Predice qué hace el límite de pasos.

## La lectura

### Reglas de las que el modelo no puede escaparse hablando

Un guardrail es una validación en tu código, fuera del modelo. Un prompt puede pedirle al modelo que siga una regla, y un mensaje astuto puede convencerlo de saltársela. Al código que rechaza una llamada no se lo puede convencer de nada.

### Cuatro tipos de validación

Las validaciones de entrada prueban el mensaje del usuario antes de que el modelo lo lea, y lo rechazan o lo marcan. Las validaciones de salida prueban la respuesta antes de que nadie la vea: por ejemplo, cada monto que menciona debe aparecer en un resultado de herramienta, para que el modelo no pueda inventar uno.

Las listas de permitidos limitan lo que puede pasar: qué herramientas pueden ejecutarse, a qué hosts pueden llegar, qué montos pueden mover. Deniegan por defecto, así que una herramienta o un argumento que nadie listó se rechaza, aunque una instrucción oculta en un ticket lo pida. Pon la lista de permitidos alrededor del nodo de herramientas, para que rechace antes de que se ejecute la herramienta.

Los límites de pasos detienen los ciclos descontrolados. recursionLimit en la config de la ejecución limita los supersteps, y LangGraph lanza GraphRecursionError cuando se alcanza. Atrápalo por err.name y conviértelo en una detención clara. Un presupuesto de tokens o de llamadas al modelo funciona igual.

### En capas

Ninguna validación sola atrapa todo. La validación de entrada nunca ve el texto que llega en un resultado de herramienta; la lista de permitidos no lee respuestas. Así que las apilas, y cuando una capa falla, la siguiente todavía puede detener el ataque.

Los modelos de seguridad dedicados son otra capa: clasificadores como los modelos de seguridad de contenido Nemotron de NVIDIA y el Llama Nemotron safety guard leen un mensaje y lo etiquetan como seguro o inseguro. Atrapan lo que se le escapa a un patrón, pero también son modelos, así que conserva además las validaciones en código.

## Pruébalo: Cuatro guardias, cuatro ataques

Lanza cuatro ataques contra un agente sin guardias. Luego activa una guardia por ataque y mira qué detiene. Termina con una repetición que detenga los cuatro.

https://agenticademy.org/es/paths/first-agents/08-guardrails

## Comprobación

El modelo con guion siempre pide otra llamada a herramienta. ¿Qué pasa?

Código:

```
await agent.invoke(
  { messages: [new HumanMessage("Check ticket T-48213")] },
  { recursionLimit: 6 },
);
```

- Se ejecuta para siempre.
- **✓** Lanza GraphRecursionError después de 6 pasos.
- Devuelve el último mensaje después de 6 pasos, sin error.
- recursionLimit solo se aplica a los subgrafos.

**Por qué:** recursionLimit limita el número de supersteps. Cuando se alcanza, LangGraph lanza GraphRecursionError, que tu código atrapa y convierte en una detención clara.

## Lo que ya puedes hacer

- Escribir validaciones de entrada y de salida alrededor de un modelo.
- Envolver un nodo de herramientas con una lista de permitidos que deniega por defecto.
- Limitar los pasos de un agente y manejar GraphRecursionError.
- Explicar por qué los guardrails van en capas.

---

[Volver a Construye tus primeros agentes](README.md)
