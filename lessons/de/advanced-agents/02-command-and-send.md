# Command und Send

Agenten für Fortgeschrittene · Kapitel 2 von 10 · 20 Minuten

Ein Knoten kann mit Command in einem Schritt den State aktualisieren und den nächsten Knoten wählen, und mit Send Arbeit auf parallele Worker verteilen, so viele, wie das Modell verlangt.

## Ziele

- Führe einen Router-Knoten aus, der den State aktualisiert und mit Command springt.
- Verteile mit Send auf so viele Prüfungen, wie das geskriptete Modell wählt, für zwei Tickets.
- Setze den Lauf fort: Der Reducer hat die Ergebnisse gesammelt, und ein Knoten fasst sie zusammen.
- Sag voraus, womit der sammelnde Knoten läuft.

## Der Text

### Command: aktualisieren und routen in einem Return

Ein Knoten gibt meist ein State-Update zurück, und die Kanten des Graphen entscheiden, was als Nächstes läuft. Stattdessen kann ein Knoten new Command({ update, goto }) zurückgeben. update wird in den State geschrieben wie bei einem normalen Return, und goto nennt den nächsten Knoten, gewählt, während der Knoten läuft. Ein Router kann ein Ticket einordnen, die Kategorie speichern und in einem Schritt zum richtigen Knoten springen.

Wohin ein Command gehen darf, deklarierst du mit ends: addNode("triage", triage, { ends: ["plan", "reply"] }). compile() prüft mit ends, dass jeder Knoten erreichbar ist: Hier haben plan und reply keinen anderen Eingang, also wirft es ohne ends UnreachableNodeError. getGraphAsync() zeichnet ends als Kanten. Ein goto kommt zu den statischen Kanten des Knotens hinzu; es ersetzt sie nicht, darum hat ein Knoten, der Command zurückgibt, meist keine.

### Send: so viele Worker, wie das Modell verlangt

new Send("check", { ticket, check }) startet einen Lauf eines Knotens, einen Task, mit eigenem Input statt des States des Graphen. Eine bedingte Kante, die eine Liste von Sends zurückgibt, verteilt die Arbeit: Alle diese Tasks laufen im nächsten Superstep, gleichzeitig. Die Liste kann vom Modell kommen, also wird die Breite zur Laufzeit festgelegt: drei Prüfungen für ein Ticket, fünf für ein anderes. Auch das goto von Command nimmt Sends an.

### Die Ergebnisse sammeln

Alle Tasks schreiben denselben Key, also braucht dieser Key einen Reducer, wie im letzten Kapitel. Am Ende des Supersteps führt der Reducer ihre Schreibvorgänge in Task-Reihenfolge zusammen, also in der Reihenfolge der Sends, egal in welcher Reihenfolge die Tasks fertig wurden. Ein Knoten mit einer Kante von den Workern läuft dann einmal, im nächsten Superstep, und liest sie alle. Das ist Map-Reduce in einem Graphen.

### Übergaben, und dieses Lab

In einem Subgraphen springt new Command({ graph: Command.PARENT, goto }) zu einem Knoten des Eltern-Graphen. So übergibt ein Agent Arbeit an einen anderen; das nächste Kapitel baut damit Teams.

Im Lab führt der Ticket-Schalter von Halden, dem erfundenen Shop aus dem letzten Kapitel, einen einzigen Graphen aus: triage routet jedes Ticket, plan lässt das geskriptete Modell die Prüfungen wählen, und summarise schreibt die Antwort. Die Seite pausiert Läufe mit interruptAfter und interruptBefore, übergeben an stream(), weil interrupt() AsyncLocalStorage braucht, das der Browser-Build von LangGraph.js nicht hat.

## Probier es aus: Routen, verteilen, sammeln

Drei Schritte auf einem LangGraph.js-Graphen: ein Ticket mit Command routen, auf die Prüfungen verteilen, die das Modell wählt, dann den Lauf fortsetzen, um zu sammeln und zusammenzufassen.

https://agenticademy.org/de/paths/advanced-agents/02-command-and-send

## Check

Das geskriptete Modell hat drei Prüfungen gewählt, und jede Prüfung dauert unterschiedlich lange. Wie oft läuft summarise, und was schreibt es?

Code:

```
// findings: a ReducedValue whose
// reducer concatenates lists.
// plan returns { checks:
//   ["order", "shipping", "stock"] }
new StateGraph(State)
  .addNode("plan", plan)
  .addNode("check", async ({ check }) =>
    ({ findings: [check] }))
  .addNode("summarise", (s) => ({
    answer: s.findings.join(" + "),
  }))
  .addEdge(START, "plan")
  .addConditionalEdges("plan", (s) =>
    s.checks.map((c) =>
      new Send("check", { check: c })),
    ["check"])
  .addEdge("check", "summarise")
  .addEdge("summarise", END)
  .compile();
```

- Dreimal, einmal pro fertiger Prüfung: erst ein Name, dann zwei, dann alle drei.
- **✓** Einmal, nach allen dreien: "order + shipping + stock", in der Reihenfolge der Sends.
- Einmal, mit allen drei Namen in der Reihenfolge, in der die Prüfungen fertig wurden, also kann es sich von Lauf zu Lauf ändern.
- Einmal: "stock". Der Schreibvorgang jeder Prüfung ersetzt den vorherigen.

**Warum:** Die drei Sends starten drei check-Tasks im selben Superstep. Wenn er endet, führt der Reducer ihre Schreibvorgänge in Task-Reihenfolge zusammen, also in der Reihenfolge der Sends, nicht in der, in der sie fertig wurden. summarise hat eine Kante von check, also läuft es einmal, im nächsten Superstep, und liest alle drei. Ohne den Reducer würden drei Schreibvorgänge auf findings in einem Superstep fehlschlagen.

## Was du jetzt kannst

- In einem Schritt routen und den State aktualisieren, indem ein Knoten Command zurückgibt, mit seinen Zielen in ends deklariert.
- Mit Send auf eine Breite verteilen, die das Modell zur Laufzeit wählt.
- Parallele Ergebnisse mit einem Reducer sammeln und ihre Reihenfolge vorhersagen, und wann der nächste Knoten läuft.

---

[Zurück zu Agenten für Fortgeschrittene](README.md) · [Weiter: Subgraphen und Teams](03-subgraphs-and-teams.md)
