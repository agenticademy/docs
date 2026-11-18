# Gastar cómputo

Agentes avanzados · Capítulo 8 de 10 · 20 minutos

Los bucles evaluador-optimizador, la reflexión, best-of-n y la búsqueda en árbol gastan más llamadas al modelo para lograr una mejor respuesta, y cada uno necesita un presupuesto.

## Objetivos

- Ejecuta un bucle de generador y juez con una rúbrica y un tope de rondas.
- Genera varias respuestas y elige una por votación.
- Busca en un pequeño árbol de planes con un presupuesto de expansiones.
- Aprueba la comprobación.

## La lectura

### Más llamadas para una mejor respuesta

Una llamada al modelo da una respuesta. Más llamadas pueden dar una mejor: un juez califica borradores, se comparan varias muestras o una búsqueda mira hacia adelante. Cada patrón multiplica las llamadas, el costo y la espera, así que cada uno necesita un presupuesto que lo detenga.

### Un generador y un juez

Un bucle evaluador-optimizador tiene dos roles. El generador escribe un borrador. El juez es una llamada al modelo aparte, con su propio prompt: puntúa el borrador según una rúbrica, una lista corta de criterios puntuados cada uno en una escala, y escribe notas para el siguiente borrador. Un umbral dice cuándo un borrador aprueba, por ejemplo cada criterio con 4 o más sobre 5. Un tope de rondas limita el bucle: cuando se alcanza, el bucle devuelve el mejor borrador hasta ese momento, marcado como no aprobado.

La reflexión, o Self-Refine, es el mismo bucle con el modelo criticando su propio borrador. Un juez aparte es más fácil de mantener estricto y de probar. Como en Sistemas de agentes, capítulo 8, compara sus puntuaciones con las de personas antes de confiar en él.

### Best-of-n y votación

Best-of-n pide n respuestas al mismo prompt, muestreadas con una temperatura mayor que 0 para que difieran, y se queda con una: un juez elige la mejor, o una votación se queda con la respuesta final más común. Votar sobre las respuestas finales se llama autoconsistencia (self-consistency); sirve para respuestas que puedes comparar con exactitud, como un monto. En LangGraph las muestras son n Sends en un superstep: n llamadas que se ejecutan a la vez, así que solo esperas a la más lenta.

### Búsqueda en árbol

Una búsqueda en árbol planea pasos por adelantado. Expande un paso en posibles pasos siguientes, le da a cada uno una estimación de valor, qué tan prometedor parece, y sigue expandiendo el paso abierto más prometedor. Language Agent Tree Search (LATS, 2023) lo hace con búsqueda en árbol de Monte Carlo, usando el modelo para proponer pasos, estimar valores y reflexionar sobre los fallos. Un presupuesto de expansiones limita cuántos pasos expande. El laboratorio ejecuta una búsqueda más pequeña, primero el mejor, en dos niveles, con valores simulados.

### Cuándo no vale la pena

Las llamadas extra compensan cuando importa una mejor respuesta y algo puede distinguir lo mejor de lo peor: una rúbrica, una respuesta exacta sobre la que votar, un valor que estimar. Si una llamada ya aprueba, o nada puede comprobar la calidad, solo suman costo y tiempo. Mídelo: ejecuta tus casos de evaluación con y sin el patrón.

## Pruébalo: Tres formas de gastar más llamadas

El ticket T-48213 de Halden, de tres formas: un bucle con juez pule la respuesta, una votación elige el monto del reembolso y una búsqueda en árbol planea los pasos. Cada una se detiene en su presupuesto.

https://agenticademy.org/es/paths/advanced-agents/08-spending-compute

## Comprobación

¿Qué devuelve esta ejecución, y tras cuántas llamadas al modelo (una por borrador, una por evaluación)?

Código:

```
const CAP = 3;
const PASS = 4; // per criterion, of 5
const scores = (r) =>
  Object.values(r.scores);
const passes = (r) =>
  scores(r).every((s) => s >= PASS);
const total = (r) =>
  scores(r).reduce((a, b) => a + b);

function review(state) {
  const last = state.rounds.at(-1);
  if (passes(last)) return 'finish';
  if (state.rounds.length >= CAP)
    return 'finish';
  return 'generate';
}

function finish(state) {
  const last = state.rounds.at(-1);
  const best = passes(last) ? last
    : state.rounds.reduce((b, r) =>
        (total(r) > total(b) ? r : b));
  return {
    answer: best.draft,
    passed: passes(best),
  };
}

// The judge's scores, round by round:
// 1: accurate 3, complete 2, tone 4
// 2: accurate 4, complete 4, tone 3
// 3: accurate 4, complete 3, tone 3
```

- El borrador 3, marcado como aprobado, tras 6 llamadas.
- **✓** El borrador 2, marcado como no aprobado, tras 6 llamadas.
- El borrador 3, marcado como no aprobado, tras 6 llamadas.
- Nada todavía: se ejecuta una cuarta ronda, porque ningún borrador ha aprobado.

**Por qué:** Ninguna ronda aprueba: la ronda 1 tiene complete 2, la ronda 2 tiene tone 3, la ronda 3 tiene complete 3 y tone 3. Así que review devuelve el bucle hasta que rounds.length llega a CAP, después de la ronda 3: tres borradores y tres evaluaciones, 6 llamadas, y ninguna cuarta ronda. Luego finish se queda con el mejor total, el borrador 2 con 11 frente al borrador 3 con 10, porque una revisión puede empeorar; el último borrador no siempre es el mejor. passed es false, así que quien llama puede marcar el borrador o pasárselo a una persona.

## Lo que ya puedes hacer

- Construir un bucle evaluador-optimizador con un juez aparte, una rúbrica, un umbral para aprobar y un tope de rondas.
- Ejecutar muestras de best-of-n como Sends en un superstep y quedarte con la respuesta en la que coincide la mayoría.
- Explicar cómo una búsqueda en árbol expande, puntúa y propaga valores con un presupuesto de expansiones.
- Decidir cuándo las llamadas extra valen su costo, y medirlo con tus casos de evaluación.

---

[Volver a Agentes avanzados](README.md) · [Siguiente: Un harness de deep agent](09-deep-agent-harness.md)
