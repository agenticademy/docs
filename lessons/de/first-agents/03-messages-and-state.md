# Nachrichten und State

Bau deine ersten Agenten · Kapitel 3 von 8 · 18 Minuten

Die vier Nachrichtentypen, warum jedes Mal die ganze Unterhaltung gesendet wird und wie LangGraph State mit Reducern zusammenführt.

## Ziele

- Ändere die Systemnachricht und vergleiche die Antworten.
- Häng eine Nachricht an und ersetze eine andere über ihre id.
- Sag voraus, was der Reducer zurückgibt.

## Der Text

### Vier Nachrichtentypen

Eine Unterhaltung ist eine Liste von Nachrichten, und jede Nachricht hat einen Typ. SystemMessage enthält deine Anweisungen an das Modell. HumanMessage ist das, was der Nutzer geschrieben hat. AIMessage ist die Antwort des Modells; sie kann tool\_calls enthalten, also Anfragen, Tools auszuführen. ToolMessage bringt das Ergebnis eines Tools zurück, über tool\_call\_id mit seiner Anfrage verknüpft. Alle vier kommen aus @langchain/core/messages.

### Das Modell merkt sich nichts

Ein Chat-Modell hat zwischen Aufrufen kein Gedächtnis. Jeder Aufruf sendet die ganze Liste, und die Antwort hängt nur von dieser Liste ab. Um eine Unterhaltung fortzusetzen, hängst du die Antwort und die nächste Frage an und sendest alles noch einmal. Deshalb wächst auch die Zahl der Input-Tokens mit jeder Runde.

### State und Reducer

In LangGraph sind die Daten, die ein Graph von Schritt zu Schritt mitführt, sein State. Ein Knoten überschreibt den State nicht: Er gibt ein Update zurück, und ein Reducer entscheidet, wie das Update mit dem Vorhandenen zusammengeführt wird. Ohne Reducer ersetzt der neue Wert den alten.

Für Nachrichten bietet LangGraph MessagesAnnotation, einen State mit einem Key, messages, zusammengeführt von messagesStateReducer. Jede Nachricht bekommt eine id; der Reducer vergibt eine, wenn sie keine hat. Eine Nachricht mit neuer id wird angehängt. Eine Nachricht mit einer id, die schon in der Liste steht, ersetzt diese Nachricht an ihrer Stelle. Eine RemoveMessage mit einer id löscht diese Nachricht. So bearbeitet ein Graph eine Unterhaltung, etwa um alte Runden zu kürzen, ohne die Liste neu aufzubauen.

## Probier es aus: Der Nachrichtenstapel

Ruf das Modell auf, ändere die Systemnachricht und ruf es erneut auf, dann sende State-Updates und sieh zu, wie der Reducer Nachrichten anhängt, ersetzt und entfernt.

https://agenticademy.org/de/paths/first-agents/03-messages-and-state

## Check

Was gibt messagesStateReducer zurück?

Code:

```
messagesStateReducer(
  [new HumanMessage({ id: "a", content: "Hi" }), new AIMessage({ id: "b", content: "Hello" })],
  [new AIMessage({ id: "b", content: "Hello!" }), new HumanMessage({ id: "c", content: "Thanks" })],
);
```

- Vier Nachrichten: a, b, b, c.
- **✓** Drei Nachrichten: a, b (jetzt "Hello!"), c.
- Zwei Nachrichten: b und c.
- Es wirft einen Fehler, weil b zweimal vorkommt.

**Warum:** Das Update mit der id b ersetzt das vorhandene b an seiner Stelle, und c ist neu, also wird es angehängt.

## Was du jetzt kannst

- Die vier Nachrichtentypen richtig einsetzen.
- Erklären, warum die Unterhaltung bei jedem Aufruf gesendet wird.
- Vorhersagen, wie der Messages-Reducer von LangGraph ein Update zusammenführt.

---

[Zurück zu Bau deine ersten Agenten](README.md) · [Weiter: Dein erster Graph](04-your-first-graph.md)
