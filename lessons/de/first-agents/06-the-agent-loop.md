# Der Agent-Loop: ein minimaler ReAct-Agent

Bau deine ersten Agenten · Kapitel 6 von 8 · 20 Minuten

Ein Modell-Knoten, ein Tool-Knoten und eine bedingte Kante: der ganze Agent-Loop als LangGraph-Graph.

## Ziele

- Lass den Agenten Superstep für Superstep laufen, bis er antwortet.
- Zähl, wie oft das Modell lief.
- Bring den Lauf in die richtige Reihenfolge.

## Der Text

### Denken, handeln, beobachten, wiederholen

ReAct (reason and act) ist ein Muster für Agenten: Das Modell liest die Unterhaltung und fordert entweder Tools an oder antwortet. Fordert es Tools an, führt dein Code sie aus und fügt ihre Ergebnisse der Unterhaltung hinzu, und das Modell liest erneut. Der Loop endet, wenn das Modell antwortet, ohne ein Tool anzufordern.

### Der Loop als Graph

In LangGraph besteht der Loop aus zwei Knoten über MessagesAnnotation, dem State, der die Liste der Nachrichten hält. Der Knoten agent ruft das Modell mit den gebundenen Tools auf state.messages auf und gibt seine Antwort zurück. Der Knoten tools ist ToolNode aus @langchain/langgraph/prebuilt: Er führt jeden Tool-Call in der letzten AIMessage aus und fügt pro Call eine ToolMessage hinzu. Ungültige Argumente werden zu Fehler-ToolMessages, wie in Kapitel 5.

toolsCondition ist der Router nach dem Knoten agent: Er gibt "tools" zurück, wenn die letzte Nachricht tool\_calls hat, und END, wenn sie keine hat. Eine Kante führt von tools zurück zu agent. Das ist der ganze Agent, etwa 25 Zeilen.

LangGraph führt einen Graphen in Supersteps aus: In jedem laufen die fälligen Knoten einmal. In diesem Graphen ist immer nur ein Knoten fällig, also ist jeder Superstep ein Knotenlauf, und graph.stream(input, { streamMode: "updates" }) liefert ein Update pro Superstep. Du kannst dem Loop beim Drehen zusehen.

### Fertig gebaut, und begrenzt

createReactAgent in @langchain/langgraph/prebuilt baut denselben Graphen; es ist zugunsten von createAgent im Paket langchain veraltet. Bau ihn einmal von Hand, dann weißt du, was diese Helfer tun.

Ein Loop kann ewig laufen, wenn das Modell immer weiter Tools anfordert. recursionLimit in der Config des Laufs begrenzt die Supersteps (standardmäßig 25); ist es erreicht, wirft LangGraph GraphRecursionError. Probier unten ein Limit von 4: Dieser Lauf braucht 5 Supersteps.

## Probier es aus: Der Loop, Schritt für Schritt

Drei Schritte mit je einer Aktion und ein optionaler vierter: Sende Ticket T-48213, lass den Loop laufen, bis der Agent antwortet, zähl die Modellaufrufe und begrenze den Loop dann mit recursionLimit.

https://agenticademy.org/de/paths/first-agents/06-the-agent-loop

## Check

Bring den Lauf von Ticket T-48213 in die richtige Reihenfolge.

1. Der Knoten agent schickt die Aufgabe an das Modell.
2. Das Modell fordert get\_ticket an.
3. ToolNode führt get\_ticket aus und fügt eine ToolMessage hinzu.
4. Das Modell liest sie und fordert list\_charges an.
5. ToolNode führt list\_charges aus und fügt eine ToolMessage hinzu.
6. Das Modell antwortet ohne Tool-Calls.
7. toolsCondition leitet zu END.

**Warum:** Jeder Zug des Modells fordert entweder Tools an, was durch ToolNode zurückführt, oder antwortet, was den Lauf beendet. Das Modell lief dreimal.

## Was du jetzt kannst

- Einen ReAct-Agenten als Graph mit ToolNode und toolsCondition bauen.
- Einen Lauf Nachricht für Nachricht mit streamMode "updates" verfolgen.
- Erklären, warum jeder Agent-Loop ein Rekursionslimit braucht.

---

[Zurück zu Bau deine ersten Agenten](README.md) · [Weiter: Callbacks und menschliche Freigabe](07-callbacks-and-approval.md)
