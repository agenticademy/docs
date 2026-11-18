# Decir lo que quieres

Principios de la IA · Capítulo 5 de 8 · 15 minutos

Donde un pedido deja un hueco, el modelo suele llenarlo con una suposición probable, así que un buen pedido describe el resultado, la forma de trabajar y el comportamiento que quieres, con los datos y un ejemplo que solo tú puedes dar.

## Objetivos

- Lee un pedido vago y la respuesta que recibió, y marca lo que la respuesta tuvo que suponer.
- Agrega partes al pedido una a una hasta que el verificador no encuentre huecos.
- Compara las respuestas al pedido vago y al pedido completo.
- Aprueba la prueba.

## La lectura

### Un hueco recibe una suposición

El capítulo 4 trataba de qué delegar. Este trata de cómo pedirlo. Lo que escribes a un asistente de chat de IA es un prompt: el texto del que parte el modelo, como en el capítulo 2. En un chat simple, sin acceso a tus archivos ni a tu calendario, todo lo que el modelo sabe de tu situación es lo que escribes.

Donde tu pedido deja un hueco, el modelo suele llenarlo con lo que es probable en general, igual que elige palabras probables. El resultado se lee bien, así que las suposiciones pasan fácilmente desapercibidas: una fecha que nadie fijó, una promesa que nadie hizo.

### Tres cosas que describir

Escribe tu pedido como un encargo: las instrucciones que le darías a un ayudante capaz que no sabe nada de tu situación. Vale la pena describir tres cosas.

El resultado: para quién es, su forma (una lista, un aviso, un correo) y qué tan largo debe ser.

La forma de trabajar: los pasos a seguir y qué preguntarte antes de empezar. “Si falta un dato, pregúntame” convierte una suposición en una pregunta.

El comportamiento: cómo debe sonar y qué evitar, como promesas que no puedes cumplir.

### Lo que solo tú sabes

Dos partes más llevan lo que el modelo no puede suponer. El contexto son los datos de fondo: fechas, nombres, lo que ya se decidió. Un ejemplo muestra cómo se ve algo bueno, como un aviso que funcionó la última vez. Di qué tomar de él, como su formato, o el modelo puede copiar también su fecha vieja.

Frases como “hazlo bien” agregan largo, no información: no dicen qué significa bien.

### En el visual

Empiezas con un pedido vago de un aviso para un huerto comunitario y la respuesta que recibió, y marcas lo que esa respuesta tuvo que suponer. Luego agregas partes una a una mientras un verificador señala lo que aún falta. El verificador son unas pocas reglas fijas que se ejecutan en esta página y miran qué partes tiene tu pedido; no es una IA. Al final, envías el pedido completo y comparas las dos respuestas. El huerto y las dos respuestas se escribieron para este capítulo para mostrar el tipo de suposiciones que puede hacer un modelo.

## Pruébalo: Armar el encargo

Marca lo que un pedido vago dejó por suponer, agrega partes hasta que el verificador no encuentre huecos y luego compara las dos respuestas.

https://agenticademy.org/es/paths/ai-principles/05-saying-what-you-want

## Comprobación

### Pregunta 1 de 4

Le pides a un asistente de chat de IA, que no tiene acceso a tu calendario ni a tu correo: “Escribe un recordatorio del almuerzo de equipo del viernes”. Su respuesta dice que el almuerzo es a las 12:30 en el salón principal. Nunca diste una hora ni un lugar. ¿Qué pasó?

- Encontró el almuerzo en tu calendario y copió de ahí la hora y el lugar.
- Sabe cuándo y dónde son los almuerzos de equipo en tu trabajo, por su entrenamiento.
- **✓** Tu pedido dejó huecos, y el modelo los llenó con detalles probables.

**Por qué:** El modelo solo sabía lo que escribiste. Donde quedó un hueco, escribió lo que es probable en general, que se lee bien y puede estar equivocado. No podía buscar nada, y no sabe nada de tu trabajo a menos que se lo cuentes.

### Pregunta 2 de 4

Quieres ayuda para planear una cena de cumpleaños para tu papá. ¿Qué línea, agregada a tu pedido, le da a la IA más cosas con qué actuar?

- **✓** “Ocho personas, una de ellas vegetariana, unos 30 euros por persona, cerca de la estación. Le gustan los lugares tranquilos.”
- “Haz que sea realmente especial, el tipo de noche que va a recordar por años.”
- “Sé lo más creativo que puedas y sorpréndeme con ideas que a mí no se me ocurrirían.”

**Por qué:** La primera da datos que la IA no puede suponer: cuántas personas, quién come qué, el presupuesto, la zona y lo que a él le gusta. “Especial” y “creativo” suenan útiles pero no dicen lo que quieres, así que la IA tiene que suponer otra vez.

### Pregunta 3 de 4

¿Qué línea describe la forma de trabajar, y no el resultado ni el comportamiento?

- “Que tenga menos de 100 palabras, como una lista corta que quepa en una pantalla de celular.”
- **✓** “Primero haz una lista de los datos que vas a usar y pregúntame por los que falten.”
- “Usa un tono cálido y amable, y no prometas nada en mi nombre.”

**Por qué:** Hacer primero la lista de datos y preguntar por los huecos es una forma de trabajar la tarea. El largo y la forma pertenecen al resultado; el tono y lo que hay que evitar pertenecen al comportamiento. Vale la pena decir las tres cosas; responden preguntas distintas.

### Pregunta 4 de 4

Pegas la carta del paseo escolar del año pasado como ejemplo y pides la de este año. La nueva carta tiene la fecha y el precio del año pasado. ¿Qué debió decir tu pedido?

- “Hazla mejor que la carta del año pasado, y más actualizada.”
- Nada más: un ejemplo por sí solo le muestra al modelo qué partes conservar.
- **✓** “Copia su formato, no sus datos. La fecha y el precio de este año están abajo.”

**Por qué:** Un ejemplo muestra cómo se ve algo bueno, pero no qué partes conservar. Di qué tomar de él, aquí el formato, y da los datos nuevos. “Mejor” y “actualizada” no dicen qué cambiar, y un ejemplo por sí solo puede copiarse entero, con los datos viejos incluidos.

## Lo que ya puedes hacer

- Explicar por qué un pedido vago recibe una respuesta llena de suposiciones.
- Describir el resultado, la forma de trabajar y el comportamiento que quieres de una IA.
- Agregar el contexto y un ejemplo que solo tú puedes dar, y decir qué tomar del ejemplo.

---

[Volver a Principios de la IA](README.md) · [Siguiente: Revisar lo que recibes](06-checking-what-you-get.md)
