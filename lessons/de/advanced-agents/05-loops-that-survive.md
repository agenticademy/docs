# Schleifen, die durchhalten

Agenten für Fortgeschrittene · Kapitel 5 von 10 · 20 Minuten

Retry-Policies, Timeouts, Error-Handler und Checkpoints halten einen langen Lauf am Leben, wenn ein Tool ausfällt oder der Prozess stoppt.

## Ziele

- Bau ein unzuverlässiges Tool ein und sieh dir die Retries an.
- Leite den Fehler zu einem Kompensationsschritt.
- Stopp den Lauf mittendrin und setze ihn ab seinem Checkpoint fort.
- Sag die Versuche und den Weg voraus.

## Der Text

### Retries und Timeouts

Ein Tool-Aufruf kann kurz scheitern: ein 503 von einem ausgelasteten Dienst oder gar keine Antwort. In LangGraph.js 1.5.1 kann ein Knoten eine retryPolicy tragen: maxAttempts (der erste Versuch zählt mit), initialInterval und backoffFactor für die Wartezeiten zwischen den Versuchen und retryOn, um die Fehler zu wählen, die einen weiteren Versuch wert sind. Ein Timeout macht aus einem hängenden Aufruf einen gescheiterten Versuch. runTimeout begrenzt die Gesamtzeit eines Versuchs; idleTimeout begrenzt die Zeit ohne Fortschritt, etwa einen State-Write, ein Callback-Event oder einen heartbeat()-Aufruf. Beide werfen NodeTimeoutError, den die Retry-Policy wie jeden anderen Fehler behandelt.

### Error-Handler und Kompensation

Wenn die Versuche aufgebraucht sind, läuft der errorHandler des Knotens, eingeführt in LangGraph.js 1.4, statt den Lauf scheitern zu lassen. Er bekommt den Eingabe-State des Knotens und einen NodeError, der den Namen des Knotens und den Fehler enthält, und gibt ein State-Update oder ein Command({ goto }) zurück. Zur Kompensation zu leiten ist das Saga-Pattern: ein Schritt, der Arbeit von vor dem Fehler rückgängig macht, etwa ein Zimmer freigeben, das reserviert, aber nie bezahlt wurde.

### Stoppen und fortsetzen

Mit einem Checkpointer endet jeder Superstep damit, dass ein Checkpoint im Thread gespeichert wird. Gib in der Config des Laufs ein RunControl mit und ruf requestDrain() auf: Die Knoten, die schon laufen, werden fertig, der Checkpoint wird gespeichert und der Lauf wirft GraphDrained. Ein neuer Aufruf mit null als Eingabe auf derselben thread\_id beginnt beim nächsten Knoten; fertige Knoten laufen nicht noch einmal. Mit einem Checkpointer, der in eine Datenbank statt in den Speicher schreibt, klappt das auch aus einem neuen Prozess.

Ein Absturz warnt nicht so. Der Knoten, der gerade lief, verliert seine Writes und läuft beim Fortsetzen noch einmal. Ein Aufruf mit Nebenwirkungen sollte deshalb einen Idempotency-Key tragen: einen Key, den sich der Dienst merkt, sodass eine wiederholte Anfrage das erste Ergebnis bekommt, statt zweimal zu handeln.

### In diesem Lab

Ein Buchungsgraph: Das Modell plant die Reise, dann rufen reserve, charge und confirm jeweils einen Dienst auf. Browser haben kein AsyncLocalStorage, deshalb können interrupt() und task() aus der Functional API hier nicht laufen. Das Lab nutzt die Graph API und reicht die Config von Hand an jeden Aufruf weiter. Sein zweiter Prozess ist ein neuer Graph auf demselben MemorySaver im Speicher, und Stecker ziehen löst das AbortSignal des Laufs aus.

## Probier es aus: Scheitern, erholen, fortsetzen

Mach ein Tool unzuverlässig und sieh dir die Retries an, leg den Zahlungsdienst lahm und folge dem Weg zur Kompensation, dann stopp den Prozess und setze den Thread fort.

https://agenticademy.org/de/paths/advanced-agents/05-loops-that-survive

## Check

charge\_card wirft bei jedem Aufruf einen 503. Was passiert, wenn das hier läuft?

Code:

```
const graph = new StateGraph(Trip)
  .addNode("reserve", reserve)
  .addNode("charge", charge, {
    retryPolicy: {
      maxAttempts: 2,
      initialInterval: 100,
    },
    errorHandler: () =>
      new Command({ goto: "release" }),
  })
  .addNode("release", release)
  .addEdge(START, "reserve")
  .addEdge("reserve", "charge")
  .addEdge("charge", END)
  .addEdge("release", END)
  .compile();

await graph.invoke(input);
```

- **✓** charge\_card wird zweimal aufgerufen; dann schickt der errorHandler den Lauf zu release, und invoke kehrt normal zurück.
- charge\_card wird dreimal aufgerufen, der erste Versuch und zwei Retries; dann läuft release.
- charge\_card wird zweimal aufgerufen; dann wirft invoke den 503.
- Der errorHandler läuft nach dem ersten 503, und dann wird charge noch einmal versucht.

**Warum:** maxAttempts zählt jeden Versuch, auch den ersten, also wird charge\_card zweimal aufgerufen, nicht dreimal. Der errorHandler läuft erst, wenn diese Versuche aufgebraucht sind, nie zwischen ihnen. Sein Command schickt den Lauf zu release, das bei END endet, also gibt invoke den State zurück. Bei einem Knoten ohne errorHandler wirft invoke den 503, sobald seine Versuche aufgebraucht sind.

## Was du jetzt kannst

- Einem Knoten eine retryPolicy und einen Timeout geben und voraussagen, wie viele Versuche er macht.
- Den letzten Fehler eines Knotens mit errorHandler und Command zu einem Kompensationsschritt leiten.
- Einen Lauf mit RunControl.requestDrain stoppen und ab seinem Checkpoint fortsetzen, ohne fertige Knoten zu wiederholen.
- Idempotency-Keys nutzen, damit ein Knoten, der nach einem Absturz noch einmal läuft, nicht zweimal handelt.

---

[Zurück zu Agenten für Fortgeschrittene](README.md) · [Weiter: Schleifen, die anhalten](06-loops-that-stop.md)
