# Dein erster Graph

Bau deine ersten Agenten · Kapitel 4 von 8 · 20 Minuten

Einen StateGraph aus Knoten und Kanten bauen, mit einer bedingten Kante routen und ihn Schritt für Schritt ausführen.

## Ziele

- Verbinde START, classify, beide Schalter und END zu einem Graphen, der kompiliert.
- Führ den Graphen so aus, dass jeder Zweig einmal genommen wird.
- Sag voraus, wie der Graph läuft.

## Der Text

### Knoten und Kanten

Ein StateGraph beginnt mit einer State-Definition, etwa Annotation.Root({ … }) oder der MessagesAnnotation aus Kapitel 3. addNode(name, fn) fügt einen Knoten hinzu: eine Funktion, die den State liest und ein Update zurückgibt, das die Reducer des States einarbeiten. addEdge(a, b) legt fest, dass b nach a läuft. START und END sind Ein- und Ausgang des Graphen: Eine Kante von START wählt den ersten Knoten, und eine Kante zu END beendet den Lauf.

### Zweige

addConditionalEdges(from, router, targets) setzt eine Entscheidung hinter einen Knoten. Der Router ist eine Funktion des States, die den Namen des nächsten Knotens zurückgibt, und targets listet die Knoten, die er zurückgeben darf. So leitet ein Graph eine Erstattungsanfrage an einen Schalter und eine Frage an einen anderen. Ohne Router laufen zwei einfache Kanten aus einem Knoten beide.

### Kompilieren, dann ausführen

compile() prüft die Struktur und gibt einen Graphen zurück, den du mit invoke() oder stream() ausführen kannst. Es wirft einen Fehler, wenn etwas nicht stimmt, zum Beispiel bei einem Knoten, den keine Kante erreicht (UnreachableNodeError). Es verlangt nicht, dass jeder Knoten END erreicht: Ein Knoten ohne ausgehende Kante beendet den Lauf einfach dort.

Ein Lauf geht in Supersteps vor. In jedem laufen alle Knoten, die bereit sind, gemeinsam; dann werden ihre Updates in den State eingearbeitet, und die Kanten entscheiden, welche Knoten als Nächstes bereit sind. stream() meldet das Update jedes Knotens, sobald es passiert. Wenn zwei Knoten in einem Superstep denselben Key schreiben, braucht dieser Key einen Reducer, der sie kombiniert, sonst wirft LangGraph einen Fehler.

## Probier es aus: Verdrahte einen Graphen

Zieh die fünf Kanten von START bis END; LangGraph kompiliert den Graphen nach jeder einzelnen. Dann führ beide Beispieltickets aus und beobachte jeden Superstep.

https://agenticademy.org/de/paths/first-agents/04-your-first-graph

## Check

Wie läuft dieser Graph?

Code:

```
new StateGraph(State)
  .addNode("a", a).addNode("b", b).addNode("c", c)
  .addEdge(START, "a")
  .addEdge("a", "b").addEdge("a", "c")
  .addEdge("b", END).addEdge("c", END)
  .compile();
```

- a, dann b, dann c: drei Supersteps.
- **✓** a, dann b und c gemeinsam in einem Superstep: zwei Supersteps.
- a, b und c alle auf einmal.
- Es kompiliert nicht: Ein Knoten kann keine zwei ausgehenden Kanten haben.

**Warum:** Wenn a fertig ist, sind b und c beide bereit, also laufen sie im selben Superstep, und ihre Updates werden zusammengeführt.

## Was du jetzt kannst

- Einen StateGraph mit Knoten, Kanten, START und END bauen und kompilieren.
- Mit addConditionalEdges routen.
- Supersteps erklären und wann Knoten parallel laufen.

---

[Zurück zu Bau deine ersten Agenten](README.md) · [Weiter: Tools und Tool-Calls](05-tools-and-tool-calls.md)
