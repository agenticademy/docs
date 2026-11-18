# Planes, y agentes que trabajan juntos

Agentes para todos · Capítulo 5 de 8 · 14 minutos

Los trabajos grandes se dividen en pasos, y a veces se reparten entre varios agentes dirigidos por uno.

## Objetivos

- Elige los pasos que pueden ejecutarse al mismo tiempo.
- Ejecuta el trabajo con un agente y luego con un equipo.
- Supera la comprobación.

## La lectura

### Un plan es una lista de pasos

Dale a un agente un objetivo grande, como organizar un almuerzo de equipo, y primero hace un plan: divide el objetivo en pasos más pequeños. Algunos pasos dependen de otros. No puedes reservar un restaurante antes de saber el día. Otros pasos son independientes y pueden ocurrir al mismo tiempo. De los pasos que se ejecutan al mismo tiempo se dice que se ejecutan en paralelo.

### Un agente, o un equipo

Un solo agente puede recorrer todo el plan por su cuenta, un paso tras otro. Todo lo que lee en el camino, cada respuesta y cada resultado, se acumula en su contexto, el texto que lee el modelo en cada paso (capítulo 4). Un trabajo largo puede llenarlo.

Otra forma es un equipo de agentes. Un agente principal, llamado orquestador, guarda el plan y entrega cada parte a un especialista: otro agente con sus propias instrucciones, sus propias herramientas y su propio contexto. Pasar una parte del trabajo de un agente a otro se llama traspaso. El especialista hace su parte y devuelve un resultado corto. El orquestador nunca lee los detalles, solo los resúmenes.

### Lo que cuesta un equipo

Un equipo puede terminar antes, porque los pasos independientes se ejecutan en paralelo, y el contexto de cada agente se mantiene pequeño y centrado en una sola cosa. Pero cada agente hace sus propias llamadas al modelo, así que un equipo suele costar más. Y cada traspaso es un lugar más donde un detalle se puede perder o malinterpretar.

Un equipo vale la pena cuando un trabajo tiene partes grandes que no dependen unas de otras. Para un trabajo pequeño, un agente es más simple y más barato.

### Palabras de este capítulo

Plan: la lista de pasos hacia un objetivo. En paralelo: al mismo tiempo. Orquestador: el agente principal que divide el trabajo y combina los resultados. Especialista: un agente al que se le da una parte del trabajo. Traspaso: pasar una parte del trabajo, con un encargo corto, de un agente a otro.

## Pruébalo: Un agente o un equipo

Marca los pasos que pueden empezar de inmediato, al mismo tiempo. Luego ejecuta el mismo trabajo con un agente y con un equipo, y compara los marcadores.

https://agenticademy.org/es/paths/agents-for-everyone/05-plans-and-teams

## Comprobación

### Pregunta 1 de 3

¿Qué hace un agente orquestador?

- Hace cada paso él mismo.
- **✓** Divide el trabajo, entrega partes a agentes especialistas y combina sus resultados.
- Comprueba que internet funcione.

**Por qué:** Un orquestador dirige: planifica, entrega partes del trabajo a especialistas y junta sus resultados. Un solo agente que hace cada paso por sí mismo es la otra forma de trabajar.

### Pregunta 2 de 3

¿Cuándo ayuda más un equipo de agentes?

- **✓** Cuando el trabajo tiene partes grandes que no dependen unas de otras.
- Para una pregunta de un solo paso.
- Siempre: más agentes siempre es mejor.

**Por qué:** Las partes independientes pueden ejecutarse en paralelo, cada una con un contexto pequeño y centrado. Un trabajo de un paso no gana nada con un equipo y aun así paga los traspasos.

### Pregunta 3 de 3

¿Cuál es un costo real de usar varios agentes?

- No pueden usar herramientas.
- **✓** Más llamadas al modelo, y más traspasos donde algo puede salir mal.
- Olvidan el objetivo por diseño.

**Por qué:** Cada agente hace sus propias llamadas al modelo, así que la cuenta crece, y cada traspaso puede perder o alterar un detalle. En el marcador, el equipo hizo más del doble de llamadas.

## Lo que ya puedes hacer

- Explicar qué es un plan y por qué algunos pasos pueden ejecutarse en paralelo.
- Describir un orquestador y sus agentes especialistas.
- Comparar un agente con un equipo para un trabajo dado.

---

[Volver a Agentes para todos](README.md) · [Siguiente: Estándares, con palabras simples](06-standards.md)
