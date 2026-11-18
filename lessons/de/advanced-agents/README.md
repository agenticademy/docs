# Agenten für Fortgeschrittene

Fortgeschritten · Für Entwickler · 10 Kapitel · 205 Min.

Für Entwickler, die schon Agentensysteme bauen können. Du arbeitest mit großen Graphen und ihrem State, routest mit Command und Send, spulst Läufe zurück und verzweigst sie, baust Loops, die sich von Fehlern erholen und wissen, wann sie aufhören, vergleichst Planungsmuster und lässt einen Deep-Agent-Harness eine lange Aufgabe erledigen. Jedes Lab führt echtes LangGraph.js 1.5.1 in deinem Browser aus, mit einem geskripteten Modell. Der Deep-Agent-Harness ist nachgebaut, und die Oberfläche sagt das.

## Bevor du anfängst

Der Lernpfad „Agentensysteme und Standards“ oder dasselbe Wissen: Graphen, Tools, die Agentenschleife, Checkpoints und Traces.

## Am Ende kannst du

- Graph-State mit Reducern und Overwrite entwerfen und vorhersagen, wie parallele Schreibvorgänge zusammenkommen.
- Mit Command und Send routen und Teams aus Subgraphen bauen.
- Einen Lauf von seinen Checkpoints aus zurückspulen, bearbeiten und verzweigen.
- Loops bauen, die wiederholen, kompensieren, nach einem Absturz weiterlaufen und bei Limits, Budgets oder auf das Wort einer Person anhalten.
- Ein Muster für Planung oder Rechenaufwand wählen und einen Deep-Agent-Harness eine lange Aufgabe erledigen lassen.
- Eine lange Aufgabe mit Übergabedateien, Kompaktierung und einem Store über mehrere Sitzungen führen und ihren Verlauf mit Regeln im Code bewerten.

## Kapitel

1. [State im großen Maßstab](01-state-at-scale.md): Wie LangGraph.js 1.5.1 Schreibvorgänge auf einen State-Key zusammenführt: StateSchema-Felder mit und ohne Reducer, die Reihenfolge paralleler Schreibvorgänge, Overwrite und UntrackedValue.
2. [Command und Send](02-command-and-send.md): Ein Knoten kann mit Command in einem Schritt den State aktualisieren und den nächsten Knoten wählen, und mit Send Arbeit auf parallele Worker verteilen, so viele, wie das Modell verlangt.
3. [Subgraphen und Teams](03-subgraphs-and-teams.md): Baue Teams von Agenten aus Subgraphen, entscheide, wie sie den State teilen, lies ihre Namespaces im Stream und vergleiche Multi-Agent-Muster nach Modellaufrufen und Tokens.
4. [Zeitreise](04-time-travel.md): Jeder Superstep speichert einen Checkpoint, also kannst du die Geschichte eines Laufs lesen, ab einem früheren Schritt neu abspielen, einen früheren State bearbeiten und ein zweites Ende abzweigen.
5. [Schleifen, die durchhalten](05-loops-that-survive.md): Retry-Policies, Timeouts, Error-Handler und Checkpoints halten einen langen Lauf am Leben, wenn ein Tool ausfällt oder der Prozess stoppt.
6. [Schleifen, die anhalten](06-loops-that-stop.md): Eine Schleife braucht Stopps, die greifen, egal was das Modell tut: ein Rekursionslimit, ein Budget an Aufrufen, einen Detektor für wiederholte Aufrufe und einen Menschen, der Nein sagen kann.
7. [Planungsschleifen](07-planning-loops.md): Plan-and-execute, ReWOO und LLMCompiler planen die Arbeit, bevor sie sie erledigen, und unterscheiden sich in Modellaufrufen, Supersteps und Zeit.
8. [Rechenleistung einsetzen](08-spending-compute.md): Evaluator-Optimizer-Schleifen, Reflexion, Best-of-n und Baumsuche geben mehr Modellaufrufe aus, um eine bessere Antwort zu bekommen, und jede davon braucht ein Budget.
9. [Ein Deep-Agent-Harness](09-deep-agent-harness.md): Ein Deep Agent plant mit einer Todo-Liste, hält Notizen in virtuellen Dateien fest, gibt Teile einer Aufgabe an Subagenten mit frischem Kontext ab und hält vor den Tools an, die du nennst.
10. [Agenten mit langem Horizont](10-long-horizon.md): Ein Job, der sich über mehrere Sessions erstreckt, braucht Compaction innerhalb einer Session, Übergabedateien und Memory zwischen den Sessions und eine Prüfung seiner ganzen Trajektorie.

https://agenticademy.org/de/paths/advanced-agents
