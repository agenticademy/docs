# Cómo escribe un modelo

Principios de la IA · Capítulo 2 de 8 · 14 minutos

Un modelo de lenguaje escribe un token a la vez sorteándolo de una lista de probabilidades, la temperatura cambia cómo sortea y nada en ese paso comprueba si las palabras son ciertas.

## Objetivos

- Lee las probabilidades para un prompt y deja que el modelo tome la palabra más probable.
- Elige tú las siguientes cinco palabras.
- Escribe con una temperatura baja y con una alta, y mira cómo se desvía el texto.
- Supera la comprobación.

## La lectura

### Una pieza a la vez

En el capítulo anterior, un clasificador aprendió de ejemplos a separar puntos por tipos. Un modelo de lenguaje, el tipo de IA que está detrás de los asistentes de chat, aprendió de una enorme cantidad de texto a hacer una sola cosa: dado un texto, decir qué es probable que venga después. Escribe repitiendo ese paso. Calcula una probabilidad para cada pieza siguiente posible, elige una, la agrega al texto y vuelve a empezar.

### Tokens y probabilidades

Las piezas se llaman tokens. Un token es un trozo corto de texto: una palabra, parte de una palabra o un signo de puntuación. Una probabilidad es un número del 0 % al 100 %, y las probabilidades de todos los tokens siguientes posibles suman 100 %. Después de “La cafetería abre a las 8 de la”, el token “mañana” recibe una probabilidad alta y “luna”, una mínima. El texto del que partes se llama prompt.

### Temperatura

El modelo sortea el siguiente token, ponderado por las probabilidades, así que los tokens probables salen más a menudo. La temperatura es un ajuste que redistribuye las probabilidades antes del sorteo. Cerca de 0, el token más probable gana casi siempre, y el mismo prompt da casi el mismo texto cada vez. Más arriba, los tokens poco probables se eligen más a menudo: el texto varía más y, pasado cierto punto, se desvía hacia el sinsentido.

### Fluido no es lo mismo que cierto

Cada token se elige porque es probable que venga después, no porque sea cierto. Nada en este paso comprueba un dato. Un modelo puede escribir una frase fluida con una fecha, un precio o un nombre equivocados, y una temperatura baja no lo arregla: solo elige las palabras más probables. Revisar lo que escribe sigue siendo tu trabajo, y un capítulo posterior muestra cómo.

### El modelo de esta página

El explorador de abajo ejecuta un modelo diminuto que tu navegador entrena al abrir la página, con diez fábulas de Esopo. Sus tokens son palabras enteras y signos de puntuación. Solo mira los dos últimos y cuenta qué vino después de ellos en las fábulas. Los modelos que están detrás de los asistentes de chat usan tokens que pueden ser partes de palabras, tienen en cuenta mucho más texto y tienen miles de millones de parámetros, los números que ajustan mientras aprenden. El paso de elegir funciona igual.

## Pruébalo: Explorador de la siguiente palabra

Lee las probabilidades para un prompt, elige tú algunas palabras y luego mueve la temperatura y mira cómo se desvía el texto.

https://agenticademy.org/es/paths/ai-principles/02-how-a-model-writes

## Comprobación

### Pregunta 1 de 3

Escribes “Nuestro tren a la costa sale a las” y un modelo continúa el texto. ¿Qué hace?

- Busca el horario y copia la hora.
- **✓** Da a cada token siguiente posible una probabilidad, sortea uno, lo agrega y luego vuelve a hacer lo mismo.
- Escribe toda la respuesta de una vez y luego la compara con los hechos.
- Siempre elige el token más sorprendente, para que el texto sea interesante.

**Por qué:** Un modelo de lenguaje escribe un token a la vez y sortea cada uno según probabilidades. No busca nada salvo que esté conectado a una herramienta, no escribe todo de una vez ni comprueba datos, y los tokens probables son los que más se eligen, no los sorprendentes.

### Pregunta 2 de 3

Le haces a un modelo la misma pregunta dos veces con una temperatura alta. ¿Qué deberías esperar?

- Exactamente la misma respuesta las dos veces.
- **✓** Respuestas que pueden ser distintas, porque cada token se sortea entre probabilidades redistribuidas.
- Un error, porque no se permite una temperatura alta.
- Respuestas más precisas, porque el modelo prueba más opciones.

**Por qué:** Una temperatura alta iguala las probabilidades, así que cada sorteo puede salir distinto y dos respuestas pueden diferir. Cerca de 0 serían casi iguales. Una temperatura alta es un ajuste permitido y no hace que las respuestas sean más precisas: solo hace que los tokens poco probables salgan más a menudo.

### Pregunta 3 de 3

Un modelo escribe, con fluidez y sin dudar: “La cafetería de Elm Street abre a las 7:30 los domingos.” ¿Qué te dice la redacción fluida sobre la hora?

- Es correcta, o el modelo no la escribiría con tanta fluidez.
- Es correcta si la temperatura era baja.
- **✓** Nada seguro: las palabras se eligieron porque eran probables, así que confirma la hora con la cafetería.
- Es incorrecta, porque los modelos siempre inventan horarios.

**Por qué:** Una redacción fluida muestra que los tokens eran probables, no que sean ciertos. Una temperatura baja solo hace que el modelo elija los tokens más probables, que aun así pueden estar equivocados, y los modelos tampoco se equivocan siempre. Comprueba datos como los horarios de apertura con una fuente de confianza.

## Lo que ya puedes hacer

- Explicar que un modelo de lenguaje escribe un token a la vez y sortea cada uno según probabilidades.
- Decir qué cambia la temperatura y qué no.
- Explicar por qué una respuesta fluida y segura puede estar equivocada.

---

[Volver a Principios de la IA](README.md) · [Siguiente: Fuerte aquí, débil allá](03-strong-here-weak-there.md)
