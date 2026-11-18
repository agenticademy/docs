# Qué es la IA y qué no es

Principios de la IA · Capítulo 1 de 8 · 12 minutos

La mayoría de los sistemas de IA que se usan hoy aprenden patrones a partir de ejemplos y los usan para inferir un resultado para algo nuevo. Un sistema así no es una mente, ni una base de datos, ni un buscador, y sus ejemplos dan forma a cada respuesta.

## Objetivos

- Coloca ejemplos de dos tipos y deja que el clasificador los aprenda.
- Prueba una fruta nueva y mira qué ejemplos deciden.
- Agrega un lote sesgado de ejemplos y mira cómo se mueve la frontera.
- Supera la comprobación.

## La lectura

### Aprender de ejemplos

La mayoría de los sistemas de IA que se usan hoy son programas que aprenden patrones a partir de ejemplos y luego usan esos patrones para producir un resultado para algo nuevo. Los ejemplos se llaman datos. El resultado puede ser una etiqueta como “spam”, un número como las ventas de la próxima semana o la siguiente palabra de una oración.

Obtener un resultado de esta manera se llama inferencia: la respuesta sale de los patrones, no de buscarla en algún lugar.

### Entrenamiento, luego uso

Un sistema así tiene dos fases. En el entrenamiento, se le muestran muchos ejemplos y se ajusta para encajar con ellos. En el uso, aplica lo que aprendió a entradas que nunca ha visto. Usarlo no cambia lo que aprendió; para eso hace falta otra ronda de entrenamiento.

### Lo que no es

No es una mente: calcula resultados a partir de patrones y no quiere nada. No es una base de datos: una base de datos devuelve solo lo que se guardó, mientras que un sistema de IA responde para entradas que nadie guardó, y puede equivocarse. No es un buscador: no trae páginas, aunque algunas herramientas lo conectan a uno.

### Los ejemplos deciden

Un sistema aprende solo de sus ejemplos, así que estos dan forma a cada respuesta. Si los ejemplos están sesgados, es decir, si un tipo o una fuente tiene muchos más que el resto, las respuestas se inclinan hacia el mismo lado. Un sistema que aprendió sobre todo de fotos tomadas en un país suele funcionar peor con fotos de otros lugares.

### En la visualización

Entrenas un clasificador, un programa que separa cosas por tipos, para distinguir limones de naranjas por su tamaño y su color. Cada fruta es un punto en un mapa. Para juzgar una fruta nueva, busca los 3 ejemplos más cercanos a ella en el mapa, sus vecinos más cercanos, y responde con el tipo al que pertenece la mayoría de ellos. La línea donde su respuesta cambia de un tipo al otro es su frontera.

Para este clasificador sencillo, entrenar significa guardar los ejemplos. Los modelos grandes, en cambio, ajustan una enorme cantidad de valores internos mientras se entrenan, y guardan esos valores en lugar de los ejemplos.

## Pruébalo: Enseña a un clasificador diminuto

Coloca limones y naranjas en el mapa, prueba una fruta que no ha visto y luego agrega un lote sesgado y mira cómo se mueve la frontera.

https://agenticademy.org/es/paths/ai-principles/01-what-ai-is

## Comprobación

### Pregunta 1 de 3

Un sistema se entrenó con miles de fotos etiquetadas como “gato” o “perro”. Ahora etiqueta una foto que nunca ha visto. ¿Qué está haciendo?

- Buscar la foto en un almacén de respuestas.
- **✓** Inferir una etiqueta a partir de patrones en sus ejemplos.
- Buscar la misma foto en la web.
- Pensar en la foto como lo haría una persona.

**Por qué:** Aplica patrones de sus ejemplos etiquetados a una entrada nueva: eso es inferencia. Un almacén de respuestas solo podría devolver fotos que ya tenía, una búsqueda en la web encuentra páginas que existen, y el sistema tiene patrones, no los pensamientos de una persona.

### Pregunta 2 de 3

Una herramienta que clasifica la bandeja de entrada de soporte aprendió de muchos mensajes de facturación y solo unos pocos de entregas. Llega un mensaje nuevo poco claro: podría ser de cualquiera de los dos. ¿Cuál es el resultado probable?

- **✓** Es más probable que lo clasifique como un mensaje de facturación.
- Es igual de probable que lo clasifique como cualquiera de los dos, porque vio ambos tipos.
- Se niega a clasificar el mensaje.

**Por qué:** Sus ejemplos se inclinan hacia la facturación, así que cerca de la frontera sus respuestas también se inclinan hacia ese lado, igual que el lote de naranjas arrastró la frontera en la visualización. Vio entregas, pero demasiado pocas para sostenerse. Y un clasificador como el de la visualización siempre elige uno de sus tipos: no tiene forma de negarse.

### Pregunta 3 de 3

En la visualización, ¿por qué el clasificador empezó a llamar naranja a un limón típico?

- El limón cambió de tamaño y de color.
- **✓** El lote puso naranjas más cerca de él, así que la mayoría de sus vecinos más cercanos pasaron a ser naranjas.
- Alguien le dijo al clasificador que la respuesta correcta era naranja.
- Más ejemplos siempre hacen que un clasificador sea menos preciso.

**Por qué:** El limón y la regla (deciden los 3 más cercanos) siguieron iguales; solo cambiaron los ejemplos. Nadie le dio la respuesta. Tener más ejemplos no es el problema en sí: el problema son los ejemplos desequilibrados.

## Lo que ya puedes hacer

- Explicar con palabras sencillas que un sistema de IA infiere resultados a partir de patrones en sus ejemplos.
- Distinguir el entrenamiento (cuando un sistema aprende de sus ejemplos) del uso (cuando aplica lo que aprendió).
- Decir por qué un sistema de IA no es una mente, ni una base de datos, ni un buscador.
- Explicar por qué ejemplos sesgados llevan a respuestas sesgadas.

---

[Volver a Principios de la IA](README.md) · [Siguiente: Cómo escribe un modelo](02-how-a-model-writes.md)
