# Schleifen, die anhalten

Agenten für Fortgeschrittene · Kapitel 6 von 10 · 20 Minuten

Eine Schleife braucht Stopps, die greifen, egal was das Modell tut: ein Rekursionslimit, ein Budget an Aufrufen, einen Detektor für wiederholte Aufrufe und einen Menschen, der Nein sagen kann.

## Ziele

- Starte einen Agenten, der einen Tool-Aufruf wiederholt, bis GraphRecursionError ihn stoppt.
- Schalte ein Aufrufbudget und einen Wiederholungsdetektor scharf und sieh, welcher zuerst auslöst.
- Pausiere vor einem riskanten Tool und lehne den Aufruf ab.
- Besteh den Check.

## Der Text

### Eine Schleife braucht Stopps, die das Modell nicht wählt

Eine Agentenschleife endet, wenn das Modell antwortet, ohne nach einem Tool zu fragen. Ein Turn ist ein Modellaufruf plus die Tools, die er anfordert. Fragt das Modell immer weiter, etwa um einen Status zu prüfen, der sich nie ändert, ist jeder Turn ein weiterer bezahlter Modellaufruf, und nichts Neues kommt dazu. Also fügst du Stopps hinzu, die greifen, egal was das Modell tut.

### Die Obergrenze von LangGraph: recursionLimit

In einer Schleife wie dieser ist jede Ausführung eines Knotens ein Superstep. recursionLimit in der Config des Laufs begrenzt die Supersteps, standardmäßig auf 25. Darüber hinaus wirft LangGraph.js GraphRecursionError, mit dem lc\_error\_code GRAPH\_RECURSION\_LIMIT. Das ist ein Auffangnetz: Es zählt Supersteps, keine Turns, es stoppt spät, und ohne Checkpointer geht der State des Laufs mit dem Fehler verloren.

### Stopps als Middleware

Middleware sind Hooks, die createAgent aus LangChain 1.6 um den Modellaufruf herum ausführt; beforeModel- und afterModel-Hooks werden jeweils zu einem Knoten des Graphen. modelCallLimitMiddleware ist ein Budget in Modellaufrufen: runLimit zählt einen Lauf, threadLimit einen ganzen Thread, und exitBehavior 'end' beendet den Lauf mit einer Nachricht. toolCallLimitMiddleware zählt die Aufrufe eines Tools oder aller Tools. Ein Budget in Tokens oder Geld funktioniert genauso: zählen, vergleichen, stoppen. Diese Seite baut beide nach, als modelCallLimit und toolCallLimit; das Lab nutzt modelCallLimit.

Ein Wiederholungsdetektor erkennt eine festgefahrene Schleife früh: dasselbe Tool mit denselben Argumenten, threshold-mal in den letzten Aufrufen. Der repeatDetector hier stammt von dieser Seite; LangChain hat keinen eingebauten. Mit exitBehavior 'end' beendet er den Lauf; mit 'continue' lehnt er den Aufruf ab und sagt dem Modell, es solle etwas anderes versuchen. Hier schreibt jeder Hook außerdem ein Trace-Event, damit du siehst, welcher Stopp ausgelöst hat.

### Ein Mensch als Stopp

Manche Aufrufe sollten auf ein Ja warten, etwa wenn Geld verschickt wird. interruptBefore pausiert den Graphen vor einem Knoten, und ein Checkpointer hält den pausierten State fest. Der Breakpoint hält vor jeder Ausführung dieses Knotens, also wählt dein Code die Aufrufe aus, die einen Menschen brauchen, und zeigt sie an. Zum Ablehnen beantwortet updateState den Aufruf mit einer ToolMessage, als wäre der Knoten tools gelaufen; ein erneutes Invoke mit null als Eingabe setzt den Lauf fort. In Node erledigt humanInTheLoopMiddleware das mit interrupt(), das AsyncLocalStorage braucht; Browser haben das nicht, deshalb nutzt diese Seite den Breakpoint.

## Probier es aus: Drei Wege, eine Schleife zu stoppen

Ein geskripteter Agent prüft eine Erstattung, die offen bleibt. Stopp ihn mit recursionLimit, dann mit einem Budget und einem Wiederholungsdetektor, dann mit einem Menschen vor dem riskanten Tool.

https://agenticademy.org/de/paths/advanced-agents/06-loops-that-stop

## Check

Das Modell fordert in jedem Turn check\_refund({ order: '4471' }) an und antwortet nie. Welcher Stopp beendet diesen Lauf?

Code:

```
const agent = createAgent({
  model,
  tools: [checkRefund],
  middleware: [
    modelCallLimit({
      runLimit: 3,
      exitBehavior: 'end',
    }),
    repeatDetector({
      window: 6,
      threshold: 4,
      exitBehavior: 'end',
    }),
  ],
});
// recursionLimit: 25 (the default)
await agent.graph.invoke({
  messages: [task],
});
```

- GraphRecursionError, bei Superstep 25.
- Der Wiederholungsdetektor, beim vierten identischen Aufruf.
- **✓** Das Aufrufbudget, vor dem vierten Modellaufruf.
- Keiner: Ein Lauf endet nur, wenn das Modell antwortet.

**Warum:** Das Aufrufbudget erlaubt drei Modellaufrufe; vor dem vierten beendet modelCallLimit den Lauf. Der Wiederholungsdetektor sieht also nie einen vierten identischen Aufruf. Jeder Turn braucht hier fünf Supersteps, also endet der Lauf bei Superstep 16, weit vor den 25 von recursionLimit. Und ein Stopp wartet nie auf das Modell: Genau dafür ist er da.

## Was du jetzt kannst

- recursionLimit als Auffangnetz setzen und GraphRecursionError abfangen.
- Ein Aufrufbudget und einen Wiederholungsdetektor als Middleware hinzufügen und am Trace-Event ablesen, welcher ausgelöst hat.
- Mit interruptBefore vor einem riskanten Tool pausieren und den Aufruf mit updateState ablehnen.

---

[Zurück zu Agenten für Fortgeschrittene](README.md) · [Weiter: Planungsschleifen](07-planning-loops.md)
