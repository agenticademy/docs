# Un modelo responde. Un agente actúa.

Agentes para todos · Capítulo 1 de 8 · 10 minutos

La diferencia entre un modelo de IA, que te da texto, y un agente, que da pasos para alcanzar un objetivo.

## Objetivos

- Envía la misma petición a un modelo y a un agente, y deja que ambos terminen.
- Encuentra el paso en el que el agente cambió algo fuera de la conversación.
- Supera la comprobación.

## La lectura

### Un modelo escribe

Un modelo de IA, como el que hay detrás de un chatbot, es un programa entrenado con una cantidad enorme de texto. Dadas unas palabras, predice las palabras que deberían venir después. Eso basta para responder una pregunta, resumir una carta o redactar un correo.

Los modelos los crean empresas y grupos de investigación. Algunos modelos son abiertos: cualquiera puede descargarlos y ejecutarlos. Muchos se comparten en Hugging Face, un sitio web para compartir modelos, y algunos, como la familia Nemotron de NVIDIA, se pueden probar gratis, con límites.

Por sí solo, un modelo solo puede devolverte texto. No puede mirar tu calendario ni reservar nada. Lo que sugiera, igual tienes que hacerlo tú.

### Un agente hace

Un agente es un programa construido alrededor de un modelo. Le das un objetivo y puede usar herramientas: otros programas que buscan información o cambian cosas, como un calendario, un mapa o un sitio de reservas.

El agente trabaja por pasos. El modelo decide qué hacer después, el agente usa la herramienta y el modelo lee lo que volvió. Esto se repite hasta alcanzar el objetivo, o hasta que el agente te necesita. “IA agéntica” es el nombre general de los sistemas que funcionan así.

### Misma petición, dos resultados

Pídele a un modelo que reserve una mesa para seis el sábado y te dice cómo reservarla. Pídeselo a un agente y puede revisar tu calendario, encontrar un restaurante, pedirte que confirmes y hacer la reserva. La simulación de abajo muestra ambos, para que puedas compararlos.

### Por qué importa la diferencia

El texto es fácil de ignorar. Las acciones no: una reserva, un pago o un correo enviado tienen efectos en el mundo. Por eso un buen agente pregunta antes de hacer algo que importa, y por eso esta ruta dedica un capítulo entero a la seguridad.

### Cuatro palabras para esta ruta

Modelo: el programa que lee y escribe texto. Herramienta: algo que un agente puede usar para buscar información o para actuar. Objetivo: lo que le pediste al agente. Paso: una decisión del modelo y una acción del agente.

## Pruébalo: Una petición, dos resultados

Haz la misma petición a un modelo y luego a un agente. Responde al agente cuando te pregunte y luego encuentra el paso que cambió algo en el mundo.

https://agenticademy.org/es/paths/agents-for-everyone/01-model-or-agent

## Comprobación

### Pregunta 1 de 4

Le pides una receta a un chatbot y te la escribe. ¿Eso es un agente trabajando?

- Sí, porque usó IA.
- **✓** No. Devolvió texto y no se hizo nada por ti.
- Sí, porque entendió lo que querías decir.

**Por qué:** Escribir una receta es lo que un modelo hace por sí solo. Un agente usaría herramientas para actuar, por ejemplo revisar qué hay en tu pedido del supermercado en línea y agregar lo que falta.

### Pregunta 2 de 4

¿Qué tiene un agente que un modelo por sí solo no tiene?

- Una memoria más grande de internet.
- **✓** Herramientas que puede usar y un ciclo de pasos que sigue hasta alcanzar el objetivo.
- Una voz y una cara.

**Por qué:** Un agente es un modelo más herramientas más un ciclo: decidir, actuar, leer el resultado, decidir de nuevo. El modelo de adentro puede ser exactamente el mismo que usa un chatbot.

### Pregunta 3 de 4

En la simulación, ¿por qué el agente te preguntó antes de reservar?

- **✓** Una reserva tiene efectos reales, así que una persona debería confirmarla primero.
- No sabía cómo reservar.
- Los agentes preguntan antes de cada paso.

**Por qué:** Buscar información es inofensivo; reservar no. Los buenos agentes preguntan antes de acciones difíciles de deshacer o que te comprometen a algo, y el resto lo hacen solos.

### Pregunta 4 de 4

¿Cuál de estas es una herramienta, en el sentido que usamos aquí?

- El texto con el que se entrenó el modelo.
- Tu petición.
- **✓** Un calendario que el agente puede leer.

**Por qué:** Una herramienta es algo que el agente puede usar durante sus pasos para buscar información o para actuar. Tu petición es el objetivo, y el texto de entrenamiento es de lo que aprendió el modelo.

## Lo que ya puedes hacer

- Distinguir un modelo de un agente con una pregunta: ¿puede actuar, o solo responder?
- Nombrar las partes de un agente: un modelo, herramientas, un objetivo y un ciclo de pasos.
- Explicar por qué un agente debería preguntarte antes de una acción que importa.

---

[Volver a Agentes para todos](README.md) · [Siguiente: El ciclo: mirar, decidir, actuar](02-the-agent-loop.md)
