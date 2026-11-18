# State im großen Maßstab

Agenten für Fortgeschrittene · Kapitel 1 von 10 · 20 Minuten

Wie LangGraph.js 1.5.1 Schreibvorgänge auf einen State-Key zusammenführt: StateSchema-Felder mit und ohne Reducer, die Reihenfolge paralleler Schreibvorgänge, Overwrite und UntrackedValue.

## Ziele

- Führ zwei parallele Knoten aus, die ohne Reducer denselben Key schreiben, und lies den Fehler.
- Füg einen Reducer hinzu und sieh, wie sich die zwei Schreibvorgänge der Reihe nach verbinden.
- Ersetze mit Overwrite einen Wert, der einen Reducer hat.
- Sag das Ende eines Supersteps voraus.

## Der Text

### Ein Superstep, viele Schreibvorgänge

LangGraph führt einen Graphen in Supersteps aus, wie in Agentensysteme, Kapitel 5. Jeder fällige Knoten läuft im selben Superstep, parallel, und gibt ein Update zurück: ein Objekt mit den State-Keys, die er ändert. Wenn alle fertig sind, wendet LangGraph die Updates an und speichert einen Checkpoint des States. Knoten in einem Superstep sehen die Updates der anderen nie, also kommt es darauf an, wie sich zwei Schreibvorgänge auf denselben Key verbinden.

### StateSchema und Reducer

Seit LangGraph.js 1.1 kannst du den State mit StateSchema deklarieren, ein Feld pro Key. Ein Feld, das als einfaches Schema angegeben ist, etwa z.string() aus Zod, hält den zuletzt geschriebenen Wert und nimmt einen Schreibvorgang pro Superstep an. Zwei Schreibvorgänge darauf im selben Superstep scheitern mit InvalidUpdateError, auch wenn die Werte gleich sind, und für diesen Superstep wird kein Checkpoint gespeichert.

Ein ReducedValue-Feld hat einen Reducer: eine Funktion (current, next) => value, die LangGraph für jeden Schreibvorgang aufruft, zum Beispiel um an eine Liste anzuhängen. Die ältere API Annotation.Root deklariert dasselbe mit Annotation({ reducer, default }).

### Welcher Schreibvorgang zuerst kommt

Ein Reducer bekommt die Schreibvorgänge eines Supersteps einzeln, in der Reihenfolge der Task-Pfade, nicht in der Reihenfolge, in der die Knoten fertig wurden. Task-Pfade werden als Strings verglichen, und der Pfad eines Knotens, den eine Kante gestartet hat, endet mit seinem Namen, also kommt hier reviews vor specs. Seit 1.5.0 nutzen Live-Läufe und Checkpointer dieselbe Reihenfolge, wie LangGraph für Python.

### Overwrite und UntrackedValue

new Overwrite(value), eingeführt in 1.2, überspringt den Reducer: Der Key wird auf value gesetzt, zum Beispiel um eine lange Liste durch eine Zusammenfassung zu ersetzen. Andere Schreibvorgänge auf diesen Key im selben Superstep werden verworfen, und zwei Overwrite-Schreibvorgänge auf einen Key in einem Superstep scheitern.

Ein UntrackedValue-Feld hält einen Wert, solange der Graph läuft: Die Knoten lesen ihn, aber kein Checkpoint speichert ihn, also startet ein Lauf, der von einem Checkpoint fortgesetzt wird, ohne ihn. Nutz es für große oder temporäre Daten, die du erneut holen kannst.

### Das Lab

Halden, ein erfundener Outdoor-Laden, fragt, ob es ein Zelt für den Winter ins Sortiment nehmen soll. Zwei Rechercheknoten, reviews und specs, fügen im selben Superstep jeweils eine Notiz zu notes hinzu; danach fasst condense die Notizen zusammen. Du führst diesen Graphen dreimal aus: ohne Reducer auf notes, mit einem Reducer, der anhängt, und mit condense, das ein Overwrite zurückgibt.

## Probier es aus: Eine Superstep-Zeitleiste

Führ denselben Graphen dreimal aus: ohne Reducer, mit einem Reducer, dann mit Overwrite. Die Zeitleiste zeigt, was jeder Superstep geschrieben und was sein Checkpoint gespeichert hat.

https://agenticademy.org/de/paths/advanced-agents/01-state-at-scale

## Check

reviews und specs folgen beide auf START, also laufen sie im selben Superstep. Was passiert, wenn er endet?

Code:

```
const State = new StateSchema({
  topic: z.string(),
  notes: new ReducedValue(z.array(z.string()).default(() => []), {
    reducer: (current, next) => current.concat(next),
  }),
});

new StateGraph(State)
  .addNode("reviews", async () => ({ notes: ["reviews"], topic: "Ridge 2" }))
  .addNode("specs", async () => ({ notes: ["specs"], topic: "Ridge 2" }))
  .addEdge(START, "reviews")
  .addEdge(START, "specs");
```

- notes ist ["reviews", "specs"] und topic ist "Ridge 2".
- **✓** Der Lauf scheitert mit InvalidUpdateError bei topic.
- Der Lauf scheitert mit InvalidUpdateError bei notes.
- topic behält den Schreibvorgang des Knotens, der zuletzt fertig war.

**Warum:** topic ist ein einfaches Feld, also nimmt es einen Schreibvorgang pro Superstep an, und es bekam zwei, obwohl beide "Ridge 2" sind. notes hat einen Reducer, also würden seine zwei Schreibvorgänge zusammengeführt, in der Reihenfolge der Task-Pfade. Kein Feld behält den Schreibvorgang, der zuletzt fertig war.

## Was du jetzt kannst

- State mit StateSchema deklarieren und für jeden Key ein einfaches Feld, ein ReducedValue oder ein UntrackedValue wählen.
- Vorhersagen, wie sich parallele Schreibvorgänge auf einen Key verbinden, in welcher Reihenfolge und wann sie scheitern.
- Mit Overwrite einen Wert ersetzen, der einen Reducer hat.

---

[Zurück zu Agenten für Fortgeschrittene](README.md) · [Weiter: Command und Send](02-command-and-send.md)
