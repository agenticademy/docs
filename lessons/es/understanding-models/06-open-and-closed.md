# Modelos abiertos y cerrados

Entender los modelos · Capítulo 6 de 8 · 15 minutos

Algunos modelos solo se pueden usar a través del servicio de su creador, otros comparten sus pesos con licencias que van de estrictas a permisivas, y unos pocos son de código abierto, con sus datos y su código.

## Objetivos

- Elige un objetivo, como usar un modelo sin conexión o lanzar una app comercial.
- Revisa los modelos nivel por nivel y mira cuáles cumplen ese objetivo.
- Abre una licencia y lee sus términos clave.
- Supera la comprobación.

## La lectura

### Cerrado: un servicio al que llamas

En el capítulo 4, los modelos abiertos publicaban sus pesos, los números que aprende un modelo, y los cerrados no. Un modelo cerrado se ejecuta en las computadoras de su creador. Llegas a él a través de la app del creador o de su API, un servicio al que tu programa envía texto, y pagas por token según los términos de servicio del creador. Claude, GPT, Gemini y Grok funcionan así.

### Pesos abiertos, con una licencia

Un modelo de pesos abiertos te deja descargar sus pesos. Puedes ejecutarlo en tu propia computadora sin conexión a internet y hacerle ajuste fino: entrenarlo más con tus propios ejemplos. Con los pesos viene una licencia, el texto legal que dice qué puedes hacer con ellos.

Una licencia permisiva, como Apache-2.0 o MIT, permite casi cualquier uso, también el comercial, siempre que conserves su aviso. Una licencia restrictiva agrega condiciones, como un acuerdo aparte por encima de cierto número de usuarios o de ingresos, un nombre que mostrar o usos que no se permiten. Gemma 4, gpt-oss y DeepSeek-V4.1-Flash tienen licencias permisivas; Llama 4 y Kimi K3 tienen licencias restrictivas.

### Código abierto

Pesos abiertos no es lo mismo que código abierto. La Open Source Initiative (OSI), que mantiene la definición de software de código abierto, escribió la Open Source AI Definition 1.0. Pide tres cosas con términos abiertos: los pesos, el código que entrena y ejecuta el modelo, e información sobre los datos, suficiente sobre los datos de entrenamiento para que una persona experta construya un modelo similar. Pocos modelos la cumplen. Olmo 3, que seguiste en el capítulo 4, publica las tres.

### Conseguirlos y ejecutarlos

Los modelos abiertos se comparten en plataformas como Hugging Face, un sitio web que guarda los archivos, la ficha de modelo y la licencia de cada modelo. Algunos tienen acceso restringido: aceptas la licencia, y a veces esperas la aprobación del creador, antes de que empiece la descarga. En tu computadora, hay apps que los ejecutan desde archivos hechos para ellas: llama.cpp y Ollama leen archivos GGUF, MLX ejecuta modelos en Macs con Apple silicon, y un navegador con WebGPU puede ejecutar los pequeños, como en el capítulo 3. Si uno cabe o no es la pregunta del capítulo 5.

### En la visualización

Eliges un objetivo y revisas modelos reales nivel por nivel, de cerrado a código abierto. Luego abres una licencia. Los datos vienen de las páginas de los modelos y de las licencias, en una instantánea del 2026-11-03. Es un resumen para aprender, no asesoría legal.

## Pruébalo: Matriz de licencias

Elige un objetivo, revisa los modelos nivel por nivel de cerrado a código abierto, y luego abre una licencia y lee sus términos clave.

https://agenticademy.org/es/paths/understanding-models/06-open-and-closed

## Comprobación

### Pregunta 1 de 3

Necesitas un modelo en una laptop sin conexión a internet. ¿Qué tipo puede hacerlo?

- Un modelo cerrado, usado a través de su API desde la laptop.
- **✓** Un modelo de pesos abiertos o de código abierto, descargado antes en la laptop.
- Cualquier modelo, siempre que tengas una clave de API para él.
- Ninguno: todos los modelos necesitan internet para responder.

**Por qué:** Funcionar sin conexión requiere los pesos en la laptop, y solo los modelos de pesos abiertos y de código abierto te dejan descargarlos. Un modelo cerrado se ejecuta en los servidores de su creador, así que su API necesita conexión, con clave o sin ella. Un modelo abierto descargado no necesita ninguna, si la laptop tiene la memoria para él.

### Pregunta 2 de 3

Los pesos de un modelo se publican con Apache-2.0, pero no se dice nada de sus datos de entrenamiento. ¿En qué nivel está?

- Cerrado, porque no se dice nada de sus datos de entrenamiento.
- Pesos abiertos, licencia restrictiva.
- Código abierto, según la definición de OSI.
- **✓** Pesos abiertos, licencia permisiva.

**Por qué:** Cerrado significa que los pesos se quedan con el creador, y estos están publicados, se diga lo que se diga de los datos. Apache-2.0 permite el uso comercial sin límites de usuarios ni de ingresos, así que la licencia es permisiva, no restrictiva. No es de código abierto: la definición de OSI también pide información sobre los datos y el código de entrenamiento, y una licencia permisiva sola no los da.

### Pregunta 3 de 3

La licencia de Llama 4 dice que por encima de 700 millones de usuarios activos mensuales necesitas otra licencia de Meta. Una startup con 20.000 usuarios crea una app de pago con Llama 4. ¿Qué significa esa condición para ella?

- No puede usar Llama 4 en una app de pago hasta que Meta le dé otra licencia.
- Debe pagarle a Meta por token para usar Llama 4, como haría con un modelo cerrado.
- **✓** El límite de usuarios no le aplica, pero los demás términos de la licencia ya sí.
- Nada de la licencia le aplica hasta que tenga 700 millones de usuarios mensuales.

**Por qué:** El límite empieza por encima de 700 millones de usuarios activos mensuales, así que una startup pequeña puede crear una app de pago sin otra licencia. Los demás términos aplican desde el primer usuario: la licencia de Llama 4 pide mostrar “Built with Llama” y seguir la política de uso de Meta. Los pesos abiertos no se pagan por token: los ejecutas tú.

## Lo que ya puedes hacer

- Distinguir los modelos cerrados, los de pesos abiertos con licencias restrictivas o permisivas, y el código abierto.
- Decir qué tipos de modelo sirven para un objetivo: una app comercial, el uso sin conexión, el ajuste fino o conocer los datos de entrenamiento.
- Leer los términos clave de una licencia, como los límites de usuarios e ingresos, antes de construir sobre un modelo.
- Nombrar dónde se comparten los modelos abiertos y las apps que los ejecutan en tu propia computadora.

---

[Volver a Entender los modelos](README.md) · [Siguiente: Comparar modelos](07-comparing-models.md)
