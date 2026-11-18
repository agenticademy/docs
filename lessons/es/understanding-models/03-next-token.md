# Elegir el siguiente token

Entender los modelos · Capítulo 3 de 8 · 14 minutos

Un modelo da una puntuación a cada token siguiente posible, convierte las puntuaciones en probabilidades y sortea uno; la temperatura, top-k y top-p cambian el sorteo, y los modelos base, instruct y de razonamiento se entrenan para escribir de formas distintas.

## Objetivos

- Mira los 20 tokens siguientes más probables para un prompt.
- Mueve la temperatura y top-p, y vuelve a sortear.
- Hazles la misma pregunta a un modelo base y a un modelo instruct.
- Supera la comprobación.

## La lectura

### Un token a la vez

En el capítulo anterior, el texto se convirtió en tokens. Un modelo escribe de la misma manera: lee todo el texto hasta ahora, elige un token siguiente, lo agrega al texto y vuelve a empezar. Una respuesta de 100 tokens lleva 100 de estos pasos.

### Las puntuaciones se vuelven probabilidades

En cada paso, el modelo da a cada token que conoce una puntuación, llamada logit. SmolLM2-360M, el pequeño modelo abierto de este capítulo, conoce 49.152 tokens, así que da 49.152 puntuaciones. Luego las puntuaciones se convierten en posibilidades, también llamadas probabilidades, que suman 100 %. Una puntuación más alta significa una probabilidad mayor: un punto más significa unas 2,7 veces la probabilidad.

### Un sorteo, con ajustes

Luego se sortea un token al azar, según esas probabilidades, como sacar un boleto de una bolsa donde los tokens probables tienen más boletos. Por eso el mismo prompt puede recibir respuestas distintas. Tres ajustes cambian el sorteo. La temperatura divide cada puntuación antes de que se convierta en probabilidad: por debajo de 1, los tokens probables se vuelven aún más probables; por encima de 1, las probabilidades se igualan. Top-k conserva solo los k tokens más probables. Top-p conserva los tokens más probables hasta que sus probabilidades suman p, así que un top-p de 0,9 descarta la larga cola de los poco probables. Lo que se conserva se reescala hasta sumar 100 %.

### Modelos base e instruct

Un modelo base solo aprendió a continuar texto, así que una pregunta puede recibir más preguntas. Un modelo instruct se entrenó además con conversaciones, escritas en un formato de chat que marca quién habla. Responde y luego escribe un token de fin, un token especial que significa que terminó.

### Modelos de razonamiento

Un modelo de razonamiento escribe primero su razonamiento, del mismo modo, token a token, y luego la respuesta. Al 2026-11-03, modelos cerrados como Claude Opus 5.5 y GPT-6.1 Sol ofrecen el razonamiento como un ajuste, y abiertos como DeepSeek-R1 y Olmo 3 7B Think se publican como modelos de razonamiento aparte.

### En la visualización

Los números de la visualización se grabaron con SmolLM2-360M y su versión instruct, ambos de Hugging Face. Si tu navegador tiene WebGPU, una forma de que las páginas web usen la tarjeta gráfica, también puedes descargar el modelo instruct y ejecutarlo en vivo.

## Pruébalo: Sorteo del siguiente token

Elige un prompt y lee las probabilidades; luego dales forma y sortea, un token a la vez. Por último, hazles la misma pregunta al modelo base y al modelo instruct.

https://agenticademy.org/es/paths/understanding-models/03-next-token

## Comprobación

Sorteas el siguiente token 100 veces con estos ajustes. ¿Qué obtienes?

Los ajustes:

```
Prompt: The capital of France is
Modelo: SmolLM2-360M (base)

Token siguiente   Probabilidad
Paris             20,3 %
the               19,6 %
located           10,5 %
situated           4,7 %
called             3,7 %
… 15 más, y luego todos los demás

Temperatura: 1
Top-k:       desactivado
Top-p:       0,5
```

- “Paris” las 100 veces: es el token más probable.
- “Paris” y “the”, unas 50 veces cada uno.
- **✓** “Paris”, “the” y “located”, unas 40, 39 y 21 veces.
- Cualquiera de los 49.152 tokens, cada uno tan a menudo como dice su probabilidad.

**Por qué:** Top-p conserva los tokens más probables hasta que sus probabilidades llegan a p. “Paris” y “the” suman 39,9 %, menos de 50 %; con “located”, el total es 50,4 %, así que esos tres se quedan y el resto queda fuera. Los tres se reescalan hasta 100 %: unos 40 %, 39 % y 21 %. Para que salga “Paris” cada vez haría falta un ajuste que conserve un solo token, como top-k en 1 o top-p en 0,2, y cualquier token podría salir solo con top-p en 1.

## Lo que ya puedes hacer

- Explicar cómo un modelo convierte puntuaciones en probabilidades y sortea el siguiente token.
- Decir qué hacen la temperatura, top-k y top-p con las probabilidades.
- Distinguir la salida de un modelo base de la de un modelo instruct, y decir qué agrega un modelo de razonamiento.

---

[Volver a Entender los modelos](README.md) · [Siguiente: De dónde vienen los modelos](04-where-models-come-from.md)
