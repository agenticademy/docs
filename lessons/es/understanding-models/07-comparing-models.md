# Comparar modelos

Entender los modelos · Capítulo 7 de 8 · 15 minutos

Los benchmarks y las clasificaciones te ayudan a comparar modelos, pero los márgenes de error, los ajustes de esfuerzo, el costo, las pruebas filtradas, las pruebas al tope, el programa de la prueba y quién la ejecutó cambian lo que significa una puntuación.

## Objetivos

- Lee una clasificación con sus intervalos y encuentra los empates.
- Compara un modelo en cinco ajustes de esfuerzo con un cambio a otro modelo.
- Relaciona cuatro trampas de los benchmarks con ejemplos.
- Supera la comprobación.

## La lectura

### Benchmarks y clasificaciones

Un benchmark es un conjunto fijo de tareas de prueba con una forma de puntuar las respuestas. Una clasificación ordena los modelos según esas puntuaciones. La visualización usa dos reales. En Arena, las personas envían un prompt, leen dos respuestas de modelos que no pueden ver y votan por la que prefieren; millones de votos se convierten en una puntuación para cada modelo. Artificial Analysis, una empresa independiente, ejecuta nueve benchmarks por su cuenta y los combina en un solo número, su Intelligence Index.

### Cada puntuación tiene un margen

Una puntuación que sale de votos, o de una muestra de tareas, es una estimación. Arena la escribe como una puntuación ± un margen: lo más probable es que la puntuación real esté en ese rango, llamado intervalo del 95 %. Menos votos dan un rango más ancho. Cuando los rangos de dos modelos se solapan, la clasificación no puede distinguirlos, así que trátalos como empatados, sea cual sea su puesto.

### Ajustes y costo

Muchos modelos tienen un ajuste de esfuerzo, como bajo, medio, alto o máximo: cuánto puede pensar el modelo antes de responder, como en el capítulo 3. Más esfuerzo suele dar una puntuación más alta, pero el modelo escribe más tokens, así que cada tarea cuesta más y tarda más. Artificial Analysis también muestra un costo por tarea: lo que costó ejecutar el modelo en una de sus tareas, en dólares estadounidenses. Una puntuación dice poco sin su ajuste y su costo.

### Trampas

Contaminación: las preguntas y respuestas de la prueba se filtraron a los datos de entrenamiento, así que un modelo puede recordarlas en lugar de resolverlas. Saturación: los mejores modelos sacan casi la puntuación máxima, así que la prueba ya no los separa. El harness es el programa que ejecuta la prueba, con sus instrucciones, herramientas y reintentos; un harness mejor puede subir la puntuación del mismo modelo. La puntuación propia de un creador se obtiene a su manera y puede diferir de una independiente. Deriva de versiones: las pruebas cambian de una versión a otra, así que una puntuación necesita su versión y su fecha.

### En la visualización

Las cifras de Arena y Artificial Analysis son reales, tal como se publicaron, en una instantánea del 2026-11-03. El eje de costo del gráfico es logarítmico: de 0,10 US$ a 1 US$ ocupa el mismo ancho que de 1 US$ a 10 US$, así que caben tanto los modelos baratos como los caros. Los ejemplos del último paso son inventados.

## Pruébalo: Leer puntuaciones de benchmarks

Encuentra los empates en una clasificación real, sube por la escalera de esfuerzo de un modelo y luego relaciona cuatro trampas con ejemplos inventados.

https://agenticademy.org/es/paths/understanding-models/07-comparing-models

## Comprobación

### Pregunta 1 de 3

En un ejemplo, el modelo A saca 1310 ± 8 en una clasificación y el modelo B saca 1302 ± 9. ¿Qué puedes decir?

- A es claramente mejor que B.
- **✓** Sus intervalos se solapan, así que la clasificación no puede distinguirlos.
- B es mejor, porque su intervalo es más ancho.
- Las puntuaciones deben de estar mal.

**Por qué:** El intervalo de A va de 1302 a 1318, y el de B de 1293 a 1311. Los rangos se solapan, así que la diferencia puede ser ruido: trátalos como empatados, no a A como claramente mejor. Un intervalo más ancho significa menos certeza, por ejemplo menos votos, no un modelo mejor. Y nada indica que las puntuaciones estén mal: son estimaciones con sus márgenes.

### Pregunta 2 de 3

Un informe dice que el modelo X saca 5 puntos más que el modelo Y en un índice. ¿Qué deberías preguntar antes de cambiar a X?

- Nada: 5 puntos es una victoria clara.
- **✓** Con qué ajuste de esfuerzo se ejecutó cada uno y cuánto costó cada uno por tarea.
- Cuál de los dos creadores es más grande.
- Qué modelo salió primero.

**Por qué:** En la visualización, de bajo a máximo movió cada modelo con cinco ajustes entre 20 y 29 puntos, con unas 8 a 20 veces el costo por tarea. Así que una diferencia de 5 puntos puede venir solo de los ajustes, y la puntuación más alta puede costar mucho más por tarea. El tamaño del creador y la fecha de lanzamiento no dicen nada de ninguna de las dos cosas.

### Pregunta 3 de 3

Una entrada de blog pone la puntuación del modelo A en la versión actual de una prueba junto a la del modelo B en la versión de hace un año. El número de A es más alto. ¿Qué puedes concluir?

- A es mejor, porque su número es más alto.
- B es mejor, porque se probó primero.
- **✓** Todavía poco: versiones distintas usan tareas distintas, así que los dos números no están en la misma escala.
- La versión más nueva debe de ser más fácil.

**Por qué:** Eso es deriva de versiones. Una versión nueva cambia las tareas, cuántas hay o cómo se ponderan, así que sus puntuaciones no están en la escala anterior. El 2026-11-03, por ejemplo, el índice de Artificial Analysis estaba en la versión 5.0.1, con nueve evaluaciones. Compara modelos en la misma versión, consultada en la misma fecha. Probarse primero no hace mejor a un modelo, y nada indica que la versión más nueva sea más fácil.

## Lo que ya puedes hacer

- Leer los intervalos de una clasificación y decir qué modelos no puede distinguir.
- Comparar modelos en ajustes de esfuerzo concretos, junto con su costo por tarea.
- Detectar contaminación, saturación, efectos del harness y puntuaciones propias de un creador en una afirmación.
- Pedir la versión, la fecha y quién ejecutó la prueba antes de fiarte de una puntuación.

---

[Volver a Entender los modelos](README.md) · [Siguiente: Elegir un modelo para un agente](08-choosing-for-an-agent.md)
