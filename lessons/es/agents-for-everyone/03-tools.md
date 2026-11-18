# Herramientas: cómo un agente llega al mundo

Agentes para todos · Capítulo 3 de 8 · 12 minutos

Las herramientas son la forma en que un agente busca información y hace cosas, y el modelo conoce cada herramienta solo por su etiqueta.

## Objetivos

- Dale al agente la herramienta correcta para tres peticiones.
- Mira cómo una descripción vaga lleva al agente a la herramienta equivocada.
- Supera la comprobación.

## La lectura

### Qué es una herramienta

Una herramienta es un programa que un agente puede usar para hacer algo que no puede hacer solo con texto: consultar el clima, leer un calendario, hacer una cuenta, enviar un correo. Para usarla, el agente completa los datos que la herramienta necesita (una ciudad y un día, para un pronóstico del clima) y lee lo que vuelve.

### Herramientas que leen, herramientas que cambian cosas

Algunas herramientas solo leen: buscan algo y no cambian nada. Otras escriben: cambian algo en el mundo, como enviar un correo o comprar un boleto. Un error con una herramienta de lectura cuesta una vuelta. Un error con una herramienta de escritura puede costar mucho más, y por eso los agentes cuidadosos te preguntan antes de usar una.

### La etiqueta es todo lo que ve el modelo

El modelo nunca ve el interior de una herramienta. De cada herramienta recibe una etiqueta: un nombre, una descripción corta de lo que hace y los datos que necesita. Elige comparando tu petición con esas etiquetas.

Así que las palabras de la etiqueta importan tanto como la herramienta misma. Una etiqueta clara lleva a la elección correcta. Una vaga lleva a errores, aunque la herramienta correcta esté ahí mismo.

### Menos herramientas, menos errores

Dale a un agente solo las herramientas que necesita el trabajo. Menos opciones significan menos elecciones equivocadas, y un agente no puede usar mal una herramienta que no tiene.

Abajo haces de ayudante del agente: pásale la herramienta correcta para tres peticiones. Luego cambia una etiqueta y mira qué hace el agente con ella.

## Pruébalo: Elige la herramienta correcta

Pásale al agente la herramienta correcta para tres peticiones; elige una herramienta para leer su etiqueta. Luego deja que el agente elija a partir de las etiquetas, haz vaga la etiqueta de Correo y envía la petición otra vez.

https://agenticademy.org/es/paths/agents-for-everyone/03-tools

## Comprobación

### Pregunta 1 de 3

¿Qué sabe el modelo de una herramienta cuando decide usarla?

- Cómo funciona por dentro el código de la herramienta.
- **✓** Su nombre, su descripción y los datos que necesita.
- Todo lo que la herramienta devolvió alguna vez.

**Por qué:** La etiqueta es todo lo que recibe el modelo. Por eso una descripción clara importa tanto como la herramienta misma.

### Pregunta 2 de 3

¿Cuál de estas es una herramienta de escritura, una que cambia algo?

- Consultar el clima de mañana.
- Buscar un restaurante.
- **✓** Enviar un correo.

**Por qué:** Enviar un correo cambia algo fuera de la conversación y no se puede deshacer. Consultar el clima o buscar restaurantes solo lee.

### Pregunta 3 de 3

¿Por qué darle a un agente solo las herramientas que necesita el trabajo?

- Hace al agente más lento pero más barato.
- **✓** Menos opciones significan menos elecciones equivocadas, y menos daño si ocurre una.
- Los agentes solo pueden tener tres herramientas.

**Por qué:** Un agente no puede usar mal una herramienta que no tiene. Pocas herramientas, con etiquetas claras, facilitan las buenas elecciones y abaratan las malas.

## Lo que ya puedes hacer

- Explicar qué es una herramienta y distinguir las de lectura de las de escritura.
- Explicar por qué la descripción de una herramienta influye en las elecciones del agente.
- Defender que a un agente se le den solo las herramientas que necesita.

---

[Volver a Agentes para todos](README.md) · [Siguiente: Memoria y contexto](04-memory-and-context.md)
