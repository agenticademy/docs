# Memoria y contexto

Agentes para todos · Capítulo 4 de 8 · 12 minutos

Lo que un agente puede ver en cada paso es limitado, así que lo que conserva, descarta y anota decide lo que sabe.

## Objetivos

- Llena el contexto hasta que salgan los mensajes más antiguos.
- Guarda el dato correcto para que el agente responda bien más tarde.
- Supera la comprobación.

## La lectura

### Lo que lee el modelo

Cada vez que el modelo decide un paso, lee un bloque de texto llamado contexto: tu petición, la conversación hasta ahora y lo que devolvieron las herramientas. Es todo lo que tiene. Si algo no está en el contexto, el modelo no lo sabe en ese paso.

### Un escritorio con poco espacio

El contexto tiene un límite de tamaño, contado en tokens. Un token es un trozo de palabra: una palabra corta es más o menos un token, y en inglés un token equivale a unos cuatro caracteres. Los límites varían entre modelos, de unos pocos miles de tokens a un millón o más, pero siempre hay uno.

Piensa en el contexto como un escritorio. Solo cabe cierta cantidad de papel a la vez.

### Cuando el escritorio se llena

Una conversación larga llena el escritorio. Entonces algo tiene que salir: se descartan los mensajes más antiguos, o varios se reemplazan por un resumen corto. Lo que se descarta, el agente ya no lo sabe, aunque se lo hayas dicho antes.

### Memoria: un cuaderno junto al escritorio

La memoria es otra cosa. Son notas que el agente escribe en algún lugar fuera del contexto, como un archivo, y que vuelve a leer cuando hacen falta. Una nota sobrevive cuando se despeja el escritorio, pero solo si el agente la guardó, y solo la parte que eligió anotar.

### Más no siempre es mejor

Un contexto abarrotado tarda más en leerse, cuesta más y puede enterrar el único dato que importa. Los buenos agentes dejan en el escritorio lo que necesita el siguiente paso y escriben el resto en el cuaderno.

## Pruébalo: Un escritorio con poco espacio

Llena el escritorio hasta que salgan los mensajes más antiguos y mira qué olvida el agente. Luego empieza de nuevo, guarda un dato en el cuaderno y vuelve a preguntar.

https://agenticademy.org/es/paths/agents-for-everyone/04-memory-and-context

## Comprobación

### Pregunta 1 de 3

¿Qué hay en el contexto de un agente en un paso?

- Todo lo que hay en internet.
- **✓** La petición, la conversación hasta ahora y los resultados de herramientas que todavía caben.
- Solo tu último mensaje.

**Por qué:** El modelo lee solo lo que se le pone delante en ese paso, hasta su límite de tamaño.

### Pregunta 2 de 3

El contexto está lleno y llega un mensaje nuevo. ¿Qué suele pasar?

- El agente deja de trabajar para siempre.
- **✓** Los mensajes más antiguos se descartan o se resumen para hacer espacio.
- Al modelo le crece un contexto más grande.

**Por qué:** El límite es fijo para cada modelo, así que el agente tiene que hacer espacio. Lo que descarta ya no lo sabe, a menos que haya guardado una nota.

### Pregunta 3 de 3

¿En qué se diferencia la memoria del contexto?

- **✓** La memoria son notas guardadas fuera del contexto que se vuelven a leer cuando hacen falta.
- Son lo mismo.
- La memoria es lo que el modelo aprendió en el entrenamiento.

**Por qué:** La memoria sobrevive cuando se despeja el contexto, porque vive en otro lugar. El agente tiene que elegir guardarla y volver a leerla.

## Lo que ya puedes hacer

- Explicar qué es un contexto y por qué tiene un límite.
- Decir qué pasa cuando se llena.
- Distinguir el contexto de la memoria, y decir por qué importa guardar los datos correctos.

---

[Volver a Agentes para todos](README.md) · [Siguiente: Planes, y agentes que trabajan juntos](05-plans-and-teams.md)
