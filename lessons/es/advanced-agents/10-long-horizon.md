# Agentes de largo horizonte

Agentes avanzados · Capítulo 10 de 10 · 25 minutos

Un trabajo que abarca varias sesiones necesita compactación dentro de una sesión, archivos de traspaso y memoria entre sesiones, y una revisión de toda su trayectoria.

## Objetivos

- Ejecuta la sesión 1 y lee los archivos que deja para la siguiente sesión.
- Inicia la sesión 2 solo desde los archivos y observa cómo la compactación reemplaza los mensajes antiguos por un resumen.
- Califica las llamadas a herramientas de las sesiones de código y abre una regla que la sesión 3 rompió.
- Aprueba la comprobación.

## La lectura

### Más largo que un contexto

Un trabajo de largo horizonte necesita más llamadas al modelo de las que caben en una ventana de contexto, o más de una sesión: construir una app función por función, por ejemplo. La ventana de contexto es lo máximo que un modelo lee en una llamada, contado en tokens. Aquí una sesión es una ejecución del agente en un hilo nuevo. Dos cosas mantienen ese trabajo en camino: reducir el contexto dentro de una sesión y traspasar el trabajo entre sesiones.

### Descarga a archivos y compactación

La publicación de LangChain sobre la gestión del contexto en Deep Agents (2026-01-28) describe tres capas. Un resultado de herramienta de más de 20.000 tokens va a un archivo, y el agente recibe su ruta y una vista previa. Pasado el 85 % de la ventana, las entradas de escrituras y ediciones de archivos antiguas se vuelven referencias a sus archivos. Cuando eso no basta, compactación: un modelo escribe un resumen de la intención, los artefactos y los próximos pasos, el resumen reemplaza los mensajes antiguos y la transcripción completa queda en disco. El harness recreado de este laboratorio descarga a archivos a partir de 20.000 tokens y compacta al 85 % de la ventana (summarize: { window, fraction, keep }), sin la capa intermedia.

### Archivos de traspaso y memoria

La publicación de Anthropic sobre harnesses para agentes de larga duración (2025-11-26) empieza cada trabajo con una sesión inicializadora. Esta escribe una lista de funciones en JSON con todas las funciones fallando, un archivo de progreso, un script init.sh y un primer commit de git. Cada sesión posterior lee el archivo de progreso, el log de git y la lista de funciones, ejecuta una prueba básica de extremo a extremo, trabaja en una función, pone su campo passes en true solo después de probarla, y termina con un commit y una nota de progreso. Nada más de la lista cambia. Para los datos que todo hilo necesita, el Store de LangGraph guarda memoria a largo plazo bajo un namespace, fuera de cualquier hilo.

### Reinicio o compactación

Empezar una sesión desde estos archivos es un reinicio de contexto: un contexto limpio y un traspaso por escrito. La compactación conserva un contexto y lo reduce. En la publicación de Anthropic del 2026-03-24, Sonnet 4.5 cerraba antes de tiempo a medida que se llenaba su contexto, así que ese harness reiniciaba; con Opus 4.5 pasó a la compactación.

### Calificar la trayectoria

Una trayectoria es la secuencia de llamadas a herramientas que hizo una ejecución. Una respuesta puede decir “listo” cuando no se ejecutó ninguna prueba, así que califica las llamadas con reglas en código, como las comprobaciones en código de Sistemas de agentes, capítulo 8. Lee las llamadas de la traza: después de la compactación, los mensajes ya no contienen las primeras.

## Pruébalo: Tres sesiones, un trabajo

Ejecuta la sesión 1, luego la sesión 2 solo desde sus archivos, luego la sesión 3, y califica las trayectorias de las sesiones de código.

https://agenticademy.org/es/paths/advanced-agents/10-long-horizon

## Comprobación

Una sesión de código de un trabajo largo empieza en un hilo nuevo. Ordena sus pasos como los ejecuta el harness de la publicación de Anthropic.

1. Leer progress.txt, el log de git y feature\_list.json.
2. Ejecutar la prueba de humo para comprobar que la app todavía funciona.
3. Escribir el código de una función.
4. Ejecutar las pruebas de esa función.
5. Poner passes de esa función en true en feature\_list.json.
6. Agregar una línea a progress.txt y hacer un commit.

**Por qué:** El hilo nuevo solo sabe lo que dicen los archivos, así que los lee primero. La prueba de humo detecta lo que la última sesión dejó roto antes de poner código nuevo encima. Una función pasa solo cuando sus propias pruebas pasan con el código nuevo: poner passes primero es el atajo que el verificador marcó en la sesión 3. La línea de progreso y el commit son lo que lee la siguiente sesión.

## Lo que ya puedes hacer

- Preparar un trabajo largo con una lista de funciones, un archivo de progreso y commits, para que cada sesión pueda empezar desde los archivos.
- Compactar un contexto con summarize al llegar a un umbral, y descargar a archivos los resultados de herramientas grandes.
- Guardar memoria a largo plazo en un Store de LangGraph que lee cada hilo.
- Calificar una ejecución por sus llamadas a herramientas, con reglas en código leídas de la traza.

---

[Volver a Agentes avanzados](README.md)
