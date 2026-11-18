# Orchestrierung: Supervisor, Fan-out, Subgraphen

Agentensysteme und Standards · Kapitel 5 von 8 · 25 Minuten

Muster für mehrere Agenten in einem LangGraph-Graphen: ein Supervisor, der weiterleitet, Send für parallele Arbeit, Subgraphen und der Checkpoint, den jeder Superstep speichert.

## Ziele

- Sieh zu, wie zwei Spezialisten im selben Superstep laufen.
- Öffne den Lauf eines verschachtelten Subgraphen.
- Führ dieselbe Aufgabe mit einem Agenten und mit einem Supervisor aus.
- Sag den Fan-out voraus.

## Der Text

### Supersteps

LangGraph führt einen Graphen in Supersteps aus. In jedem Superstep laufen alle fälligen Knoten parallel; ihre Updates werden dann vom Reducer jedes Keys in den State gemischt, und die Kanten entscheiden, welche Knoten als Nächstes laufen. Nachrichten nutzen einen Reducer, der anhängt, sodass zwei Knoten im selben Superstep beide Nachrichten hinzufügen können.

### Threads und Checkpoints

Kompilier einen Graphen mit einem Checkpointer, etwa MemorySaver, und gib beim Ausführen eine thread\_id mit. LangGraph speichert unter diesem Thread einen Checkpoint des ganzen States: einen für den Input, dann einen am Ende jedes Supersteps. getState({ configurable: { thread\_id } }) liest den neuesten: die Werte, die Schrittnummer und next, die Knoten, die als Nächstes laufen. Ein späterer Lauf im selben Thread startet von diesem State.

### Supervisor, Fan-out, Subgraphen

Ein Supervisor ist ein Agent, der entscheidet, welcher Spezialist als Nächstes arbeitet. Hier ist eine Übergabe ein Tool-Call wie transfer\_to\_customer\_support, also wählt das Modell sie wie jedes andere Tool.

Fan-out führt Arbeit parallel aus. Eine bedingte Kante kann statt Knotennamen eine Liste von Send-Objekten zurückgeben. Jedes Send startet einen Lauf eines Knotens mit eigenem Input, alle im nächsten Superstep, und der Reducer mischt, was sie zurückgeben.

Ein Subgraph ist ein kompilierter Graph, der in einem Knoten genutzt wird. Er hat seinen eigenen State und seinen eigenen Checkpoint-Namespace, und seine Läufe hängen unter dem Knoten, der ihn aufgerufen hat. In diesem Lab ruft der Financial Analyst einen eigenen Data-Engineer-Subgraphen auf. Übergaben mit Command und Subgraphen, die den State teilen, kommen später, im Pfad Agenten für Fortgeschrittene.

### Wann ein Agent reicht

Jeder Agent bringt Modellaufrufe, Checkpoints und Übergaben, und eine weitere Stelle, an der etwas schiefgehen kann. Teil die Arbeit auf, wenn ihre Teile unabhängig sind, wenn ein einzelner Kontext zu groß würde oder wenn ein Teil andere Tools oder Anweisungen braucht. Sonst fang mit einem Agenten an.

In diesem Lab bearbeitet Haldens Support-Team das Ticket T-48213: Ein Kunde wurde für die Bestellung 48213 doppelt belastet. Führ es auf beide Arten aus und vergleich, was die Traces messen.

## Probier es aus: Ein Himmel voller Agenten

Drei Schritte: Führ die Aufgabe als Supervisor aus und sieh zu, wie jeder Superstep einen Checkpoint speichert, öffne einen Subgraphen im Laufbaum, dann führ dieselbe Aufgabe mit einem Agenten aus und vergleich.

https://agenticademy.org/de/paths/agent-systems/05-orchestration

## Check

results hat einen Reducer, der Listen verkettet. Was gibt der Graph zurück?

Code:

```
new StateGraph(State)
  .addNode("lookup", s => ({ results: [s.id * 10] }))
  .addConditionalEdges(START, () => [new Send("lookup", { id: 1 }), new Send("lookup", { id: 2 })], ["lookup"])
  .addEdge("lookup", END)
  .compile()
  .invoke({});
```

- **✓** { results: [10, 20] }: zwei Läufe von lookup in einem Superstep.
- { results: [20] }: das zweite Send überschreibt das erste.
- { results: [10] }: nur das erste Send läuft.
- Es wirft einen Fehler: An einen Knoten kann nicht zweimal gesendet werden.

**Warum:** Jedes Send startet einen eigenen Lauf von lookup mit eigenem Input ({ id: 1 } und { id: 2 }). Beide laufen im selben Superstep, und der Reducer verkettet ihre Updates.

## Was du jetzt kannst

- Einen Supervisor bauen, der mit Übergabe-Tool-Calls an Spezialisten-Agenten weiterleitet.
- Mit Send einen Fan-out machen und Ergebnisse mit einem Reducer zusammenführen.
- Einen kompilierten Graphen als Subgraphen nutzen und sagen, wann ein Agent reicht.
- Einen Graphen in einem Thread ausführen und seinen letzten Checkpoint mit getState lesen.

---

[Zurück zu Agentensysteme und Standards](README.md) · [Weiter: Context Engineering und Retrieval](06-context-engineering.md)
