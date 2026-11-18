# De dónde vienen los modelos

Entender los modelos · Capítulo 4 de 8 · 15 minutos

Un modelo se hace por etapas, desde los datos y el preentrenamiento hasta el ajuste fino, la retroalimentación, la destilación y la cuantización, y en los modelos abiertos muchas etapas dejan un archivo que puedes revisar.

## Objetivos

- Sigue a Olmo 3, un modelo totalmente abierto, desde sus datos hasta su lanzamiento.
- Sigue a DeepSeek-R1 hasta un modelo pequeño destilado de él.
- Sigue a Gemma 4 desde su modelo base hasta un archivo cuantizado.
- Supera la comprobación.

## La lectura

### Datos, tokens y preentrenamiento

Todo modelo empieza con datos: grandes colecciones de texto, código y a menudo imágenes, filtradas y sin copias repetidas. Un tokenizador, como en el capítulo 2, los divide en tokens.

El preentrenamiento le muestra al modelo billones de tokens y ajusta sus pesos, también llamados parámetros: los números que aprende. Así mejora en predecir el siguiente token, la tarea del capítulo 3. Los laboratorios guardan checkpoints, copias de los pesos a lo largo del proceso. Algunos, como Olmo 3, terminan con un entrenamiento intermedio, una fase más corta con una mezcla más pequeña y elegida, y una fase con textos largos. El resultado es un modelo base, que continúa texto pero no sigue peticiones.

### Postentrenamiento

El postentrenamiento hace que un modelo base siga peticiones. El ajuste fino supervisado (SFT) lo entrena con ejemplos de peticiones con buenas respuestas.

Luego, el aprendizaje por refuerzo (RL) lo entrena con recompensas: las respuestas que obtienen una buena puntuación se vuelven más probables. En RLHF, RL con retroalimentación humana, las personas comparan respuestas y las puntuaciones salen de lo que eligen. En RLAIF, otro modelo da la retroalimentación, a veces guiado por una lista escrita de principios llamada constitución. DPO, la optimización directa de preferencias, aprende de pares de una respuesta mejor y una peor en un solo paso más simple.

Los modelos de razonamiento agregan RL con recompensas verificables (RLVR): una respuesta solo se recompensa cuando un programa la confirma, como un resultado matemático o un código que pasa sus pruebas.

### Más pequeños y más ligeros

La destilación entrena un modelo pequeño, el estudiante, con respuestas escritas por uno grande, el maestro. La cuantización guarda cada peso con menos bits, los 0 y 1 de la memoria de una computadora, por ejemplo 4 en lugar de 16, así que el archivo ocupa menos y cabe en máquinas más pequeñas, con una pequeña pérdida de calidad. El entrenamiento consciente de la cuantización (QAT) entrena un modelo para que pierda menos cuando se cuantiza. GGUF es un formato de archivo que leen las apps que ejecutan modelos en tu propia computadora.

### Probados y descritos

Antes del lanzamiento, los laboratorios evalúan un modelo con benchmarks, conjuntos fijos de preguntas de prueba, y le hacen red teaming: unos evaluadores intentan que haga algo dañino. Una ficha de modelo describe el modelo, su entrenamiento y sus resultados; una ficha de sistema también cubre las salvaguardas que lo rodean.

Los modelos abiertos publican sus pesos; los totalmente abiertos también publican sus datos y su código. Los modelos cerrados, como Claude, GPT, Gemini y Grok, también se entrenan, pero sus creadores publican informes, no pesos, así que sus árboles no se pueden seguir archivo por archivo. La visualización sigue tres modelos abiertos.

## Pruébalo: El árbol genealógico de un modelo

Sigue tres árboles reales desde arriba, una etapa a la vez. Cada etapa muestra qué usó y qué produjo, y dónde está publicado.

https://agenticademy.org/es/paths/understanding-models/04-where-models-come-from

## Comprobación

Ordena las etapas de hacer un modelo, de la primera a la última.

1. Reunir y limpiar los datos, y dividirlos en tokens.
2. Preentrenar en predecir el siguiente token: el resultado es un modelo base.
3. Hacerle ajuste fino con ejemplos de peticiones con buenas respuestas.
4. Entrenarlo más con recompensas: a partir de retroalimentación y de respuestas que un programa puede comprobar.
5. Destilar un modelo estudiante más pequeño a partir de sus respuestas.
6. Cuantizar los pesos del estudiante a menos bits.
7. Evaluar el resultado y hacerle red teaming, y luego publicar su ficha de modelo.

**Por qué:** El preentrenamiento lee tokens, así que los datos y los tokens van primero, y da un modelo base. El ajuste fino con ejemplos le enseña a ese modelo a seguir peticiones, y las recompensas le dan más forma, como en el SFT, el DPO y el RLVR de Olmo 3. Solo un modelo terminado puede enseñarle a un estudiante, y luego la cuantización achica el archivo del estudiante. Las últimas pruebas y el red teaming van antes del lanzamiento, y la ficha de modelo informa sobre ellos.

## Lo que ya puedes hacer

- Nombrar en orden las etapas que hacen un modelo, de los datos a la ficha de modelo.
- Explicar qué cambia cada uno: el ajuste fino, el entrenamiento con retroalimentación, la destilación y la cuantización.
- Seguir el árbol genealógico de un modelo abierto desde su ficha de modelo, etapa por etapa.
- Decir por qué un modelo totalmente abierto se puede comprobar en cada etapa y uno cerrado no.

---

[Volver a Entender los modelos](README.md) · [Siguiente: Tamaño y memoria](05-size-and-memory.md)
