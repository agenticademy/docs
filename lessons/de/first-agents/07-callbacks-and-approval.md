# Callbacks und menschliche Freigabe

Bau deine ersten Agenten · Kapitel 7 von 8 · 22 Minuten

Einen Lauf mit Callbacks beobachten und ihn vor einem riskanten Tool anhalten, damit ein Mensch ihn freigeben oder ablehnen kann.

## Ziele

- Häng einen Callback-Handler an und beobachte seine Events.
- Halte vor dem Erstattungs-Tool an und gib es dann frei.
- Lehne eine Erstattung ab und sieh, wie der Agent damit umgeht.
- Besteh den Check.

## Der Text

### Callbacks beobachten

Ein Callback-Handler ist ein Objekt, dessen Methoden LangChain im Lauf aufruft: eine Unterklasse von BaseCallbackHandler mit Methoden wie handleChatModelStart, handleLLMEnd, handleToolStart und handleToolEnd. Du übergibst ihn in config.callbacks, und er erreicht jeden verschachtelten Aufruf, der diese Config erhält. Nutze ihn für Logs, Metriken und Traces. Ein Callback beobachtet nur: LangChain ignoriert, was seine Methoden zurückgeben, und loggt einen darin geworfenen Fehler standardmäßig nur, also kann er einen Schritt weder ändern noch stoppen.

### Breakpoints halten an

Um einen Lauf vor einem Schritt anzuhalten, muss LangGraph den State speichern und später dorthin zurückkehren. Das macht ein Checkpointer: compile({ checkpointer: new MemorySaver() }) speichert nach jedem Superstep einen Checkpoint unter der thread\_id in config.configurable. Füg interruptBefore: ["approval\_tools"] hinzu, und der Lauf hält vor diesem Knoten an.

Während er pausiert, sagt (await graph.getState(config)).next, was wartet, und die letzte AIMessage enthält den ausstehenden Call. Zum Freigeben setzt du mit graph.invoke(null, config) fort: Eine null-Eingabe bedeutet „mach am Checkpoint weiter“. Zum Ablehnen rufst du zuerst graph.updateState(config, { messages: [toolMessage] }, "approval\_tools") auf: Deine ToolMessage beantwortet den Call, als wäre der Knoten gelaufen, also wird der Knoten übersprungen und das Modell liest deine Ablehnung.

Gib riskanten Tools einen eigenen Knoten, wie hier create\_refund, damit die Schranke nur sie stoppt.

### In Node und anderswo

In Node bietet createAgent aus dem Paket langchain Middleware-Hooks (beforeModel, afterModel, wrapModelCall, wrapToolCall) und humanInTheLoopMiddleware, die mit interrupt() anhält. interrupt() braucht AsyncLocalStorage, deshalb nutzt du im Browser Breakpoints, wie hier. Coding-Agenten nutzen dieselbe Idee: Hooks, die vor einem Tool-Call laufen und ihn verweigern können.

## Probier es aus: Eine Schranke vor der Erstattung

Vier Schritte mit je einer Aktion und ein optionaler fünfter: Beobachte die Callback-Events eines Laufs, setz eine Schranke vor die Erstattung, gib einen Lauf frei und lehne dann den nächsten ab.

https://agenticademy.org/de/paths/first-agents/07-callbacks-and-approval

## Check

### Frage 1 von 3

Was braucht interruptBefore, um zu funktionieren?

- **✓** Einen Checkpointer und eine thread\_id in der Config.
- Einen Callback-Handler.
- Ein echtes Modell.

**Warum:** Anhalten heißt, den State zu speichern und später dorthin zurückzukehren. Der Checkpointer speichert ihn unter der id des Threads.

### Frage 2 von 3

Der Lauf ist vor approval\_tools angehalten. Wie setzt du ihn so fort, wie er war?

- Erneut mit der ursprünglichen Eingabe aufrufen.
- **✓** graph.invoke(null, config) mit derselben thread\_id.
- Einen neuen Thread starten.

**Warum:** Eine null-Eingabe sagt LangGraph, ab dem gespeicherten Checkpoint fortzusetzen, statt einen neuen Lauf zu starten.

### Frage 3 von 3

handleToolStart eines Callback-Handlers gibt false zurück, wenn er create\_refund sieht. Was passiert?

- Die Erstattung wird blockiert.
- **✓** Nichts ändert sich: LangChain ignoriert den Rückgabewert, also läuft die Erstattung. Um einen Schritt zu stoppen, halte den Graphen an oder schütze das Tool.
- Der Lauf hält vor approval\_tools an.

**Warum:** Callbacks sind zum Beobachten und Loggen da. Die Steuerung gehört in den Graphen: ein Breakpoint mit updateState, wie hier, oder eine Prüfung im Tool-Knoten.

## Was du jetzt kannst

- Die Modell- und Tool-Events eines Laufs mit einem Callback-Handler loggen.
- Einen Graphen mit einem Checkpointer und interruptBefore vor einem Knoten anhalten.
- Einen ausstehenden Schritt mit getState, updateState und invoke(null) fortsetzen oder ablehnen.

---

[Zurück zu Bau deine ersten Agenten](README.md) · [Weiter: Guardrails](08-guardrails.md)
