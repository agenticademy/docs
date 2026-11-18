# Entender los modelos

Principiante · Sin código · 8 capítulos · 120 min

Para quien quiera saber qué hay dentro de los modelos detrás de las herramientas de IA. Ves cómo el texto se convierte en tokens, cómo un modelo elige el siguiente, cómo se hacen los modelos, cuánto ocupan y qué permiten sus licencias, y luego eliges un modelo para un agente. Los ejemplos combinan modelos cerrados con modelos abiertos, y cada cifra viene de una instantánea con fecha.

## Antes de empezar

Nada. Ayudan los dos primeros capítulos de Principios de la IA.

## Al final podrás

- Nombrar los tipos de modelos que usa un agente y qué hace cada uno.
- Explicar los tokens, cómo un modelo elige el siguiente y qué cambia la temperatura.
- Seguir cómo se hace un modelo, desde los datos y el preentrenamiento hasta un archivo cuantizado.
- Distinguir modelos cerrados, de pesos abiertos y de código abierto, y leer los términos clave de una licencia.
- Leer una tabla de clasificación con cuidado y elegir un modelo para un agente según su capacidad, su costo y sus políticas.

## Capítulos

1. [Tipos de modelos](01-kinds-of-models.md): Los modelos de chat, de razonamiento, multimodales, de embeddings, los rerankers y los modelos de voz, de imagen, guardianes y pequeños hacen trabajos distintos, y un agente suele usar varios.
2. [Tokens](02-tokens.md): Los modelos leen y escriben texto en forma de tokens, cada familia de modelos divide el texto a su manera, y tanto los precios como los límites de contexto se cuentan en tokens.
3. [Elegir el siguiente token](03-next-token.md): Un modelo da una puntuación a cada token siguiente posible, convierte las puntuaciones en probabilidades y sortea uno; la temperatura, top-k y top-p cambian el sorteo, y los modelos base, instruct y de razonamiento se entrenan para escribir de formas distintas.
4. [De dónde vienen los modelos](04-where-models-come-from.md): Un modelo se hace por etapas, desde los datos y el preentrenamiento hasta el ajuste fino, la retroalimentación, la destilación y la cuantización, y en los modelos abiertos muchas etapas dejan un archivo que puedes revisar.
5. [Tamaño y memoria](05-size-and-memory.md): La memoria que necesita un modelo sale de sus parámetros y de los bits que guardan cada uno, más espacio para su contexto; una mezcla de expertos usa unos pocos de sus parámetros en cada token, pero los mantiene todos en memoria.
6. [Modelos abiertos y cerrados](06-open-and-closed.md): Algunos modelos solo se pueden usar a través del servicio de su creador, otros comparten sus pesos con licencias que van de estrictas a permisivas, y unos pocos son de código abierto, con sus datos y su código.
7. [Comparar modelos](07-comparing-models.md): Los benchmarks y las clasificaciones te ayudan a comparar modelos, pero los márgenes de error, los ajustes de esfuerzo, el costo, las pruebas filtradas, las pruebas al tope, el programa de la prueba y quién la ejecutó cambian lo que significa una puntuación.
8. [Elegir un modelo para un agente](08-choosing-for-an-agent.md): Elegir un modelo es descartar los que no cumplen un límite estricto y luego sopesar capacidad, velocidad, precio por tarea, contexto y fiabilidad con herramientas para el trabajo de un agente.

https://agenticademy.org/es/paths/understanding-models
