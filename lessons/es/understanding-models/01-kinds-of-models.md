# Tipos de modelos

Entender los modelos · Capítulo 1 de 8 · 14 minutos

Los modelos de chat, de razonamiento, multimodales, de embeddings, los rerankers y los modelos de voz, de imagen, guardianes y pequeños hacen trabajos distintos, y un agente suele usar varios.

## Objetivos

- Completa el primer espacio del agente de voz: convertir la voz de quien llama en texto.
- Completa los dos espacios que encuentran las notas que responden la pregunta.
- Completa los tres últimos espacios: escribir la respuesta, revisarla y decirla.
- Supera la comprobación.

## La lectura

### Modelos distintos, trabajos distintos

Un modelo es un programa que aprendió patrones a partir de ejemplos. Los modelos aprenden trabajos distintos. Cada uno recibe algún tipo de entrada, como texto, sonido o imágenes, y entrega algún tipo de salida. Un agente suele encadenar varios modelos, y cada uno hace el trabajo que mejor sabe hacer.

### Modelos que escriben

Un modelo de chat, también llamado modelo instruct, sigue instrucciones y escribe una respuesta. Un modelo de razonamiento resuelve un problema paso a paso antes de responder: es mejor en problemas difíciles, pero más lento. Muchos modelos nuevos hacen ambas cosas, con un ajuste para cuánto piensan. Un modelo multimodal también lee imágenes, sonido o video.

### Modelos que encuentran y ordenan

Un modelo de embeddings convierte un texto en una lista de números, de modo que los textos con significados parecidos reciben números parecidos. Así, una búsqueda puede comparar los números y encontrar notas de significado cercano a una pregunta, rápido, incluso entre miles. Un reranker lee la pregunta junto con cada nota que recibe y pone la mejor primero. Eso es más cuidadoso pero más lento, así que solo ordena las pocas notas que encontró la búsqueda.

### Modelos que escuchan, hablan, dibujan y revisan

Un modelo de voz a texto escribe lo que alguien dice. Uno de texto a voz lee un texto en voz alta. Un modelo de imagen crea una imagen a partir de una descripción. Un modelo guardián, también llamado clasificador de seguridad, etiqueta textos o imágenes como seguros o inseguros, para que un agente pueda detener una mala respuesta antes de que alguien la vea o la oiga.

### Pequeños, cerrados y abiertos

El tamaño de un modelo se cuenta en parámetros: los valores internos que ajustó durante el entrenamiento, también llamados sus pesos. Los modelos pequeños tienen unos pocos miles de millones de parámetros o menos, así que pueden funcionar en un teléfono o una laptop. Los pesos de un modelo cerrado no se publican, y la mayoría de los modelos cerrados se usan a través de un servicio en línea. Un modelo de pesos abiertos se puede descargar y ejecutar en tu propia computadora.

En la visualización equipas un agente de voz para la línea telefónica de una biblioteca, un espacio a la vez. La biblioteca es inventada; cada modelo de ejemplo que encuentras es real, al 2026-11-03.

## Pruébalo: Arma un agente de voz

Completa un espacio a la vez, desde arriba: lee qué recibe el espacio y qué debe entregar, y luego elige la tarjeta de modelo que coincide.

https://agenticademy.org/es/paths/understanding-models/01-kinds-of-models

## Comprobación

### Pregunta 1 de 3

Un agente de soporte debe encontrar, entre 5000, los tres artículos de ayuda que mejor responden a la pregunta de un cliente. ¿Qué par de modelos está hecho para ese trabajo?

- Un modelo de imagen y un modelo de voz a texto.
- **✓** Un modelo de embeddings para encontrar artículos de significado cercano, y luego un reranker para poner el mejor primero.
- Un modelo guardián y un modelo de texto a voz.
- Un reranker solo, que lee los 5000 artículos con la pregunta.

**Por qué:** Los embeddings permiten que una búsqueda compare números, así que encuentra artículos parecidos rápido, incluso entre miles. Luego un reranker lee la pregunta con cada uno de los pocos encontrados y pone el mejor primero. Un reranker solo leería los 5000 pares en cada pregunta, lo que es lento. Los modelos de imagen, de voz, guardianes y de texto a voz hacen otros trabajos: imágenes, sonido y etiquetas de seguridad.

### Pregunta 2 de 3

Una app para trabajo de campo debe responder preguntas en un teléfono sin conexión a internet. ¿Qué tipo de modelo puede hacerlo?

- Un modelo de chat cerrado, usado a través del servicio de su creador.
- **✓** Un modelo pequeño de pesos abiertos, descargado en el teléfono.
- Un modelo de razonamiento en un servidor.
- Un modelo de embeddings solo.

**Por qué:** Sin conexión, el modelo debe funcionar en el propio teléfono. Un modelo pequeño de pesos abiertos puede hacerlo: lo descargas una vez y funciona sin internet. Un modelo de chat cerrado usado a través del servicio de su creador y un modelo de razonamiento en un servidor necesitan conexión para poder usarse. Un modelo de embeddings entrega números, no respuestas.

### Pregunta 3 de 3

Una persona le hace a un agente de voz una pregunta fácil y espera en la línea. ¿Por qué un modelo de chat encaja mejor que un modelo de razonamiento en el espacio de la respuesta?

- Un modelo de razonamiento no puede leer texto.
- **✓** Un modelo de razonamiento resuelve pasos antes de responder, así que quien llama espera más por una respuesta fácil.
- Un modelo de chat siempre tiene razón.
- Un modelo de razonamiento entrega sonido, no texto.

**Por qué:** Ambos reciben texto y entregan texto. Un modelo de razonamiento dedica tiempo a pensar antes de responder, lo que ayuda en problemas difíciles pero deja a quien llama en silencio ante uno fácil. Ningún modelo tiene siempre razón, y convertir texto en sonido es el trabajo de texto a voz.

## Lo que ya puedes hacer

- Nombrar diez tipos de modelos y decir qué recibe y qué entrega cada uno.
- Elegir un tipo de modelo para cada paso de un agente sencillo, y decir por qué otro tipo no encajaría.
- Dar un ejemplo cerrado y uno abierto de cada tipo, a partir de una instantánea con fecha.

---

[Volver a Entender los modelos](README.md) · [Siguiente: Tokens](02-tokens.md)
