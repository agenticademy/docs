# El ciclo: mirar, decidir, actuar

Agentes para todos · Capítulo 2 de 8 · 12 minutos

Todo agente ejecuta el mismo ciclo: mira lo que sabe, decide el siguiente paso, actúa y vuelve a mirar.

## Objetivos

- Toma tres decisiones como si fueras el modelo.
- Lleva al agente hasta su objetivo.
- Ordena los pasos de una vuelta.

## La lectura

### Un ciclo, una y otra vez

En el capítulo 1 viste a un agente trabajar por pasos. Cada paso sigue el mismo ciclo de tres partes.

Mirar: el agente reúne lo que tiene hasta ahora, que es tu petición y el resultado de su último paso. Decidir: el modelo lee todo eso y elige el siguiente paso, o decide que alcanzó el objetivo. Actuar: el agente da el paso, normalmente usando una herramienta, y vuelve un resultado. Luego mira de nuevo.

Una pasada por mirar, decidir y actuar se llama vuelta. Un trabajo pequeño puede llevar tres o cuatro vueltas; uno grande, docenas.

### Por qué vuelve a mirar

Lo que devuelve una herramienta es información nueva. Si una búsqueda muestra que un plomero está fuera todo el mes, la siguiente decisión puede tenerlo en cuenta. Así cambia de rumbo un agente sin que se lo digan: lee el resultado antes de volver a elegir.

### Cuándo se detiene

Un agente se detiene por una de tres razones. Alcanzó el objetivo. Te necesita: hace una pregunta o pide tu aprobación, y espera. O llega a un límite, como un número máximo de vueltas.

### Cómo fallan los ciclos

Una mala decisión cuesta una vuelta. Reservar antes de haber encontrado a nadie, por ejemplo, no logra nada. Peor es un ciclo descontrolado: el agente repite el mismo paso, obtiene el mismo resultado y lo repite otra vez. Sin un límite podría seguir para siempre, y cada vuelta usa el modelo, lo que lleva tiempo y a menudo cuesta dinero. Por eso los agentes reales cuentan sus vueltas y se detienen en un límite.

### Tu turno

Abajo, tú haces de modelo. El agente mira, luego tú decides, luego el agente actúa y te muestra lo que volvió. Prueba una o dos malas elecciones y mira lo que cuestan.

## Pruébalo: Tú eres el modelo

Tú decides cada paso del agente. Mira lo que devuelve cada acción y cómo una mala elección cuesta una vuelta extra.

https://agenticademy.org/es/paths/agents-for-everyone/02-the-agent-loop

## Comprobación

Ordena las partes de una vuelta del ciclo.

1. Leer la petición y el resultado del último paso.
2. Decidir el siguiente paso, o decidir que se alcanzó el objetivo.
3. Usar una herramienta para dar ese paso.
4. Leer lo que devolvió la herramienta.

**Por qué:** Mirar, decidir, actuar y volver a mirar: lo que devuelve la herramienta pasa a ser parte de lo que el agente mira en la siguiente vuelta.

## Lo que ya puedes hacer

- Describir el ciclo que ejecuta todo agente: mirar, decidir, actuar.
- Nombrar tres razones por las que un agente se detiene: alcanzó el objetivo, te necesita o llegó a un límite.
- Explicar por qué los agentes necesitan un límite de cuántos pasos dan.

---

[Volver a Agentes para todos](README.md) · [Siguiente: Herramientas: cómo un agente llega al mundo](03-tools.md)
