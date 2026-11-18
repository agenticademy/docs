# Tamaño y memoria

Entender los modelos · Capítulo 5 de 8 · 15 minutos

La memoria que necesita un modelo sale de sus parámetros y de los bits que guardan cada uno, más espacio para su contexto; una mezcla de expertos usa unos pocos de sus parámetros en cada token, pero los mantiene todos en memoria.

## Objetivos

- Lleva Llama-3.1-8B hasta un tipo de 4 bits y mira cómo se reduce su archivo.
- Comprueba qué versiones caben en 8, 16, 24 y 80 GB.
- Compara los parámetros totales y activos de tres modelos de mezcla de expertos.
- Supera la prueba.

## La lectura

### Los parámetros ocupan memoria

En el capítulo 1 conociste los parámetros, los números que aprendió un modelo, también llamados pesos. El nombre suele dar su cantidad: Llama-3.1-8B tiene unos 8 mil millones. Para funcionar, un modelo debe tener todos sus pesos en memoria: el almacenamiento rápido con el que trabaja una computadora, o la memoria de una tarjeta gráfica (una GPU, un chip que hace muchas sumas a la vez). La memoria se cuenta en bytes, y un byte son 8 bits. Así que los pesos necesitan parámetros × bits por peso ÷ 8 bytes. A 16 bits cada uno, 8 mil millones de pesos ocupan 16 GB, 16 mil millones de bytes.

### Menos bits, un poco peor

La cuantización, del capítulo 4, guarda cada peso con menos bits. Menos bits pueden guardar menos valores, así que cada peso se redondea de forma más gruesa: el archivo se reduce y las respuestas empeoran un poco, más aún con la menor cantidad de bits. El visual usa mediciones de llama.cpp, un programa gratuito que ejecuta modelos en tu propia computadora. Los nombres de sus tipos dan los bits: Q4\_K\_M guarda la mayoría de los pesos en 4 bits y algunos en más, más un poco extra por cada bloque de pesos: 4,89 bits en promedio. Da los tamaños en GiB: 1 GiB son 1.073.741.824 bytes, alrededor de un 7 % más que 1 GB.

### Espacio para trabajar

Un modelo cabe cuando sus pesos, más algo de espacio, caben en la memoria que tienes. En una laptop, esa memoria se comparte con todo lo demás que se ejecuta. El espacio es para el contexto: mientras un modelo trabaja, guarda lo que calculó para cada token, y ese almacén crece con cada token, hasta la ventana de contexto del capítulo 2.

### Muchos expertos, unos pocos a la vez

Un modelo de mezcla de expertos divide la mayoría de sus capas en expertos, grupos más pequeños de pesos. Para cada token, una pequeña parte llamada router elige unos pocos expertos, y solo ellos trabajan. Sus parámetros, más las partes que usa cada token, son los parámetros activos; todos juntos son el total. Un token cuesta más o menos el mismo trabajo que en un modelo del tamaño activo, pero el router puede elegir cualquier experto para el siguiente token, así que el total debe estar en memoria.

### Cerrados y abiertos

Los modelos cerrados como Claude, GPT, Gemini y Grok no publican sus pesos, y sus páginas no dan la cantidad de parámetros, así que cada cantidad de parámetros del visual viene de la página propia de un modelo abierto, a fecha de 2026-11-03.

## Pruébalo: ¿Cabe?

Lleva un modelo a menos bits, prueba cuatro tamaños de memoria y luego mira cuánto de un modelo grande trabaja en cada token.

https://agenticademy.org/es/paths/understanding-models/05-size-and-memory

## Comprobación

Este modelo de mezcla de expertos está cuantizado a unos 4,9 bits por peso. ¿Cuál es la más pequeña de estas memorias que guarda sus pesos?

El modelo:

```
Modelo:     gpt-oss-120b, pesos abiertos
Parámetros: 117 mil millones en total
Activos:    5,1 mil millones por token
Cantidades: ficha del modelo, 2026-11-03
Cuantizado: 4,9 bits por peso

Memoria:    8 GB · 16 GB · 24 GB · 80 GB
```

- 8 GB: solo los 5,1 mil millones de parámetros activos deben estar en memoria, alrededor de 3,1 GB.
- 16 GB: los 5,1 mil millones de parámetros activos a 16 bits cada uno, alrededor de 10,2 GB.
- **✓** 80 GB: los 117 mil millones de parámetros deben estar en memoria, alrededor de 72 GB.
- Ninguna: 117 mil millones × 4,9 bits son alrededor de 573 GB.

**Por qué:** El router puede elegir cualquier experto para el siguiente token, así que los 117 mil millones de parámetros deben estar en memoria, no solo los 5,1 mil millones que usa un token: 117 mil millones × 4,9 bits ÷ 8 bits por byte ≈ 72 GB, lo que deja unos 8 GB de los 80 para el contexto. 8 GB y 16 GB guardarían solo la parte activa, a 4,9 o a 16 bits. 573 GB olvida dividir entre 8, los bits de un byte.

## Lo que ya puedes hacer

- Estimar la memoria que necesitan los pesos de un modelo a partir de sus parámetros y sus bits por peso.
- Decir qué ahorra la cuantización y qué cuesta.
- Saber si un modelo cabe en una memoria dada, con espacio libre para su contexto.
- Explicar los parámetros totales y activos de una mezcla de expertos, y por qué el total fija la memoria.

---

[Volver a Entender los modelos](README.md) · [Siguiente: Modelos abiertos y cerrados](06-open-and-closed.md)
