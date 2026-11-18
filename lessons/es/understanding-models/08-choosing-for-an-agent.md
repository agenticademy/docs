# Elegir un modelo para un agente

Entender los modelos · Capítulo 8 de 8 · 18 minutos

Elegir un modelo es descartar los que no cumplen un límite estricto y luego sopesar capacidad, velocidad, precio por tarea, contexto y fiabilidad con herramientas para el trabajo de un agente.

## Objetivos

- Elige un agente para el que elegir un modelo.
- Aplica sus límites estrictos y mira qué modelos quedan fuera.
- Sopesa lo que queda, ordénalo y calcula el precio de un mes de trabajo.
- Aprueba la prueba.

## La lectura

### El que encaja, no el mejor

No hay un mejor modelo, solo el que mejor encaja con el trabajo de un agente. Elegir lleva dos pasadas. Primero vienen los límites estrictos: necesidades sin las que el agente no puede funcionar. Un modelo que no cumple uno queda fuera, por muy fuerte o barato que sea. Luego sopesas lo que queda: decides cuánto importa cada cualidad para este trabajo y ordenas los modelos según eso.

### Límites estrictos

Modalidad: los tipos de entrada y salida que maneja un modelo, como texto, imágenes o sonido. Ventana de contexto: cuántos tokens lee a la vez, como en el capítulo 2. Política de datos: si el proveedor puede entrenar sus modelos con lo que envías, y la retención, cuánto tiempo lo guarda. Región: dónde se procesan tus datos; algunas normas, como las de la UE, se fijan en dónde. Licencia y acceso: qué puedes hacer con un modelo y quién puede usarlo. Los pesos abiertos también pueden ejecutarse en tu propia máquina, incluso sin internet.

### Lo que sopesas

Capacidad: qué tan bien hace el trabajo. Los benchmarks, como en el capítulo 7, ayudan cuando sus tareas se parecen a las tuyas, pero tu propia evaluación dice más: unas cuantas decenas de tareas reales del trabajo, puntuadas por ti. Latencia: cuánto esperas una respuesta; pensar más suele dar mejores respuestas y esperas más largas.

Precio por tarea: lo que cuesta una tarea terminada. Un agente llama a su modelo varias veces por tarea, y cada llamada es un turno, cobrado por sus tokens de entrada y de salida. Un precio por token barato puede igual dar una tarea cara. Fiabilidad con herramientas: con qué frecuencia el modelo llama bien a las herramientas, con las entradas correctas. Límites de uso: cuántas solicitudes y tokens acepta un proveedor por minuto.

### En el visual

Eliges uno de cuatro agentes. Sus límites estrictos descartan modelos, tus prioridades ordenan el resto y una calculadora convierte precios de lista reales en una factura mensual. Después, una respuesta resuelta muestra a qué renuncia su elección. Los agentes y el tamaño de sus tareas son inventados. Los modelos, precios, ventanas de contexto, entradas, licencias y puntuaciones son reales, de una instantánea del 2026-11-03. La instantánea no tiene regiones y solo algunos términos de datos, así que algunos límites dicen “Verificar”; no tiene velocidades ni límites de uso, así que esos los pruebas tú.

## Pruébalo: Elige un modelo para este agente

Elige un agente, aplica sus límites estrictos uno a uno, sopesa lo que queda y calcula el precio de un mes de trabajo; luego compara con una respuesta resuelta.

https://agenticademy.org/es/paths/understanding-models/08-choosing-for-an-agent

## Comprobación

### Pregunta 1 de 3

Un bot de soporte para un banco europeo debe mantener los datos de clientes en la UE. Dos modelos pueden hacer el trabajo; el más barato solo se ejecuta en servidores fuera de la UE. ¿Qué va primero?

- Elegir el más barato: el precio es lo que más importa.
- **✓** Descartar el que no puede mantener los datos en la UE y luego comparar el resto.
- Elegir el que tiene la puntuación más alta en la clasificación.
- Usar los dos y dejar que voten.

**Por qué:** Mantener los datos en la UE es un límite estricto: un modelo que no lo cumple queda fuera, por muy barato o fuerte que sea. El precio y las puntuaciones solo ordenan los modelos permitidos. Dejar que ambos voten igual enviaría los datos fuera de la UE.

### Pregunta 2 de 3

El modelo B cuesta el doble por token que el modelo A. En tu evaluación, B termina una tarea en 2 turnos, y A necesita 5 turnos del mismo tamaño. ¿Cuál cuesta menos por tarea?

- A, porque su precio por token es la mitad del de B.
- **✓** B, porque 2 turnos al doble de precio cuestan menos que 5 turnos.
- Ninguno: con los mismos tokens por turno, decide el precio por token.
- No se puede saber sin una puntuación de clasificación.

**Por qué:** El precio por tarea es el precio por token por los tokens que usa una tarea. Si un turno cuesta 1 en A, cuesta 2 en B, así que la tarea de A cuesta 5 y la de B cuesta 4. El precio por token solo no cuenta el número de turnos, y una puntuación de clasificación no cuenta tus turnos en absoluto; tu propia evaluación sí.

### Pregunta 3 de 3

El primer modelo de tu lista corta tiene la mejor puntuación independiente, pero la instantánea no dice nada de su velocidad ni de qué tan bien llama a herramientas. ¿Qué haces antes de comprometerte?

- Nada: la mejor puntuación significa que también es el más rápido y fiable.
- **✓** Hacer tu propia evaluación: tareas reales del trabajo, cronometrando las respuestas y revisando cada llamada a herramienta.
- Elegir el modelo más barato en su lugar.
- Esperar la clasificación del mes que viene.

**Por qué:** Una puntuación mide las tareas de su benchmark con un ajuste; no cronometra respuestas en tu trabajo ni revisa tus herramientas. Tu propia evaluación, unas decenas de tareas reales que puntúas tú, mide justo eso. Elegir solo por precio, o esperar una clasificación más nueva, deja el mismo hueco.

## Lo que ya puedes hacer

- Separar los límites estrictos que descartan un modelo de las cualidades que sopesas.
- Calcular un precio por tarea y por mes a partir de tokens, turnos y precios de lista.
- Ordenar una lista corta según lo que importa para un agente y decir a qué renuncia la elección.
- Nombrar lo que la página de un modelo no puede decirte y probarlo en tu propia evaluación.

---

[Volver a Entender los modelos](README.md)
