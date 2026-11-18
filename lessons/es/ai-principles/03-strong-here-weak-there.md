# Fuerte aquí, débil allá

Principios de la IA · Capítulo 3 de 8 · 15 minutos

La IA hace bien algunas tareas difíciles y falla en otras que parecen fáciles, puede afirmar cosas falsas con seguridad y es más útil cuando dice que no sabe.

## Objetivos

- Responde seis preguntas o di “No lo sé”, y luego puntúalas de dos maneras.
- Ubica seis tareas cotidianas en un mapa con fecha de dónde a la IA le va bien y dónde falla.
- Supera la comprobación.

## La lectura

### Desigual, y no según la dificultad

En el capítulo 2 viste que un modelo escribe eligiendo los fragmentos de texto siguientes más probables, aprendidos de los ejemplos con los que se entrenó. Lo que hace bien depende de esos ejemplos, no de lo que a las personas les resulta difícil. Puede traducir un cartel a tres idiomas en segundos y luego contar mal las letras de una palabra, porque lee fragmentos de palabras y no letras sueltas.

Los investigadores llaman a esto una frontera irregular: el límite entre lo que la IA hace bien y lo que hace mal va en zigzag. La expresión viene de un estudio de 2023 en el que consultores usaron IA en tareas realistas (Dell'Acqua y colegas). En las tareas dentro de la frontera, su trabajo mejoró. En una tarea justo fuera de ella, quienes usaban IA acertaron menos a menudo que quienes trabajaban sin ella. La frontera también se mueve a medida que cambian los modelos, así que cualquier mapa de ella necesita una fecha.

### Seguro y equivocado

Un modelo siempre produce una respuesta, y sus palabras suenan igual de seguras cuando se equivoca. Escribir algo falso en un tono seguro, como una cita de un libro que no existe, se llama confabulación; mucha gente lo llama alucinación. Estar calibrado significa que tu confianza coincide con la frecuencia con la que aciertas: cuando dices que tienes certeza, casi siempre aciertas.

### Por qué adivinar conviene en la mayoría de las pruebas

Abstenerse significa decir “No lo sé” en lugar de responder. Que convenga o no depende de la regla de puntuación, la forma en que una prueba convierte las respuestas en puntos. Si solo cuentan las respuestas correctas, adivinar solo puede ayudar, así que alguien que siempre responde le gana a alguien que admite sus dudas. Un artículo de 2025 de investigadores de OpenAI sostiene que la mayoría de las pruebas que se usan para clasificar modelos de IA puntúan así, de modo que un modelo que adivina queda por encima de uno que admite que no está seguro. La solución que propone: que una respuesta incorrecta cueste más que “No lo sé”.

### En la visualización

Respondes seis preguntas; nadie podría saber algunas de las respuestas a partir de esta página. Tus respuestas se puntúan de las dos maneras, junto a dos jugadores simulados. Luego ubicas seis tareas cotidianas en un mapa con fecha: suele ir bien, revisa con cuidado o no confíes en ello.

## Pruébalo: Responder o abstenerse, y luego ubicar las tareas

Responde seis preguntas, mira cómo se puntúan de dos maneras y luego ubica seis tareas cotidianas en un mapa con fecha.

https://agenticademy.org/es/paths/ai-principles/03-strong-here-weak-there

## Comprobación

### Pregunta 1 de 3

Un cuestionario da 1 punto por una respuesta correcta, 0 por “No lo sé” y menos 1 por una incorrecta. Tienes alrededor de un 30 % de certeza en tu respuesta. ¿Qué da más puntos en promedio?

- Responder igual: adivinar vale la pena con cualquier regla de puntuación.
- **✓** Decir “No lo sé”: con un 30 % de certeza, adivinar resta puntos en promedio.
- Cualquiera de las dos: ambas dan los mismos puntos en promedio.
- Responder y agregar una explicación más larga para ganar más.

**Por qué:** Responder da 0.3 × 1 + 0.7 × (−1) = −0.4 puntos en promedio, y “No lo sé” da 0, así que las dos no valen lo mismo y abstenerse es lo que más puntos da. Adivinar solo vale la pena cuando las respuestas incorrectas no cuestan nada, y una explicación más larga no cambia los puntos. Lo mismo vale para un modelo puntuado así.

### Pregunta 2 de 3

Un asistente de chat traduce bien una carta larga al japonés y luego se equivoca en el número de letras de una palabra corta. ¿Qué muestra esto?

- Está roto, así que no se puede confiar en nada de su trabajo.
- Contar letras es más difícil que traducir, también para las personas.
- No se esforzó en la tarea fácil, como haría una persona aburrida.
- **✓** Lo que hace bien no sigue lo que a las personas les resulta difícil.

**Por qué:** Esta es la frontera irregular: un modelo puede hacer bien una tarea difícil para las personas y fallar en una que un niño puede hacer, porque lee fragmentos de palabras, no letras. Así que juzga cada tipo de tarea por separado. Un error no hace que todo su trabajo sea poco confiable, contar no es más difícil para las personas y no estaba holgazaneando: el error viene de cómo lee el texto.

### Pregunta 3 de 3

Le preguntas a un asistente de chat sin búsqueda web si la panadería de tu calle está abierta ahora mismo. Responde: “Sí, hoy está abierta hasta las 6 p. m.” ¿Qué deberías hacer?

- **✓** Tomarlo como una conjetura y revisar la página de la propia panadería, o llamar.
- Confiar en la respuesta: suena segura, así que debe saberlo.
- Preguntar de nuevo: si dice lo mismo dos veces, es correcto.
- Confiar en la respuesta, porque es corta y concreta.

**Por qué:** No tiene forma de ver el horario de hoy, así que una respuesta segura y concreta puede ser igualmente una confabulación: las palabras de un modelo suenan igual sepa o no. Preguntar de nuevo solo da otra conjetura, que puede coincidir o no. Revisa una fuente que sí lo sepa.

## Lo que ya puedes hacer

- Explicar por qué la IA puede hacer bien una tarea difícil y fallar en una que parece fácil.
- Decir por qué una respuesta segura puede estar equivocada, y por qué puntuar solo las respuestas correctas premia adivinar.
- Para una tarea cotidiana, decidir si el resultado de la IA suele ir bien, necesita revisión o no se debe confiar en él.

---

[Volver a Principios de la IA](README.md) · [Siguiente: Qué delegar](04-what-to-hand-over.md)
