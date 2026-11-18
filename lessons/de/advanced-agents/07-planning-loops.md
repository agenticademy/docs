# Planungsschleifen

Agenten für Fortgeschrittene · Kapitel 7 von 10 · 20 Minuten

Plan-and-execute, ReWOO und LLMCompiler planen die Arbeit, bevor sie sie erledigen, und unterscheiden sich in Modellaufrufen, Supersteps und Zeit.

## Ziele

- Starte Plan-and-execute: ein Plan, ein Schritt nach dem anderen und eine Neuplanung nach jedem Schritt.
- Starte ReWOO: ein Plan mit #E-Platzhaltern, die Tools, dann eine Antwort.
- Starte LLMCompiler: ein Task-Graph, dessen unabhängige Tasks gleichzeitig laufen.
- Besteh den Check.

## Der Text

### Erst planen, dann handeln

Die bisherigen Agenten haben einen Tool-Aufruf nach dem anderen entschieden: Modell aufrufen, Tool ausführen, Modell wieder aufrufen. Das ist die ReAct-Schleife. Ein planender Agent bittet das Modell zuerst um einen Plan und arbeitet ihn dann ab. Drei veröffentlichte Muster tun das auf verschiedene Weise. Das Lab vergleicht sie an drei Zählern: Modellaufrufe, Supersteps (eine Runde von Knotenläufen, wie in Kapitel 1) und wie lange ein Lauf dauert.

### Plan-and-execute

Ein Knoten planner schreibt eine Liste von Schritten. Ein Knoten agent erledigt den ersten: Das Modell wählt einen Tool-Aufruf, und der Knoten führt ihn aus. Ein Knoten replan liest, was erledigt ist, und antwortet dann oder schreibt die übrigen Schritte neu. Der Plan kann sich nach jedem Ergebnis ändern, aber jeder Schritt kostet mindestens zwei Modellaufrufe: einen zum Handeln, einen zum Neuplanen. Die Idee stammt aus dem Paper Plan-and-Solve (2023) und dem Plan-and-execute-Tutorial von LangGraph.

### ReWOO

ReWOO (2023) schreibt den ganzen Plan in einem Modellaufruf und benennt jedes Ergebnis, bevor es existiert: #E1 = search[…], dann #E3 = forecast[#E1]. #E1 ist ein Platzhalter. Ein Knoten tool führt die Schritte der Reihe nach aus und setzt jedes Ergebnis für seinen Namen ein, ohne Modellaufruf. Ein Knoten solve ruft das Modell noch einmal auf, um zu antworten. Das sind insgesamt zwei Modellaufrufe, aber der Plan kann sich nicht mehr ändern, sobald er geschrieben ist.

### LLMCompiler

LLMCompiler (2023) schreibt den Plan als Task-Graph ohne Schleifen, einen DAG: 3. forecast($1) heißt, dass Task 3 das Ergebnis von Task 1 braucht. Ein Scheduler, die Task Fetching Unit aus dem Paper, startet jeden Task, sobald die Tasks fertig sind, die er braucht, also laufen Tasks, die nichts voneinander brauchen, gleichzeitig. Ein Knoten join liest die Ergebnisse und antwortet dann oder fordert einen neuen Plan an.

### In diesem Lab

Eine Rechercheaufgabe läuft auf drei Arten: die Wettervorhersage für Samstag am Start von zwei Trailläufen, für Halden, den erfundenen Shop. Jeder Graph ist echtes LangGraph.js 1.5.1, gezeichnet aus getGraphAsync(). In Node könntest du den Scheduler auch mit der funktionalen API schreiben, als entrypoint, dessen task()-Aufrufe parallel laufen. task() braucht AsyncLocalStorage, das dem Browser-Build fehlt, also startet der Knoten schedule hier einfache Promises und reicht config von Hand weiter. Das Modell ist geskriptet, und jeder Aufruf dauert eine geskriptete Zeit, auf dem Bildschirm beschleunigt und als simulierte Zeit gezählt.

## Probier es aus: Eine Aufgabe, drei Planer

Führ dieselbe Rechercheaufgabe als Plan-and-execute, ReWOO und LLMCompiler aus und vergleiche dann ihre Modellaufrufe, Supersteps und simulierte Zeit.

https://agenticademy.org/de/paths/advanced-agents/07-planning-loops

## Check

### Frage 1 von 3

Eine Aufgabe braucht fünf Suchen, die nicht voneinander abhängen, dann eine Antwort. Welches Muster kann am frühesten fertig sein?

- Plan-and-execute: replan hält den Plan aktuell.
- ReWOO: Es ruft das Modell nur zweimal auf.
- **✓** LLMCompiler: Die fünf Suchen können gleichzeitig laufen.
- Alle drei brauchen gleich lang, weil die Suchen dieselben sind.

**Warum:** Der Scheduler von LLMCompiler startet jeden Task, dessen Eingaben bereit sind, also überlappen sich die fünf Suchen und dauern etwa so lange wie die langsamste. ReWOO ruft das Modell auch nur zweimal auf, aber sein Knoten tool führt eine Suche pro Superstep aus, eine nach der anderen. Plan-and-execute fügt pro Suche zwei Modellaufrufe hinzu. Die Suchen sind dieselben; wann sie laufen, nicht.

### Frage 2 von 3

Ein ReWOO-Plan hat den Schritt #E3 = forecast[#E1]. Was passiert mit #E1, bevor forecast läuft?

- **✓** Der Knoten tool setzt an seiner Stelle das Ergebnis von Schritt #E1 ein.
- Das Modell wird noch einmal aufgerufen, um aus #E1 einen Ort zu machen.
- forecast bekommt den Text „#E1“ und schlägt ihn nach.
- Der Knoten solve füllt ihn am Ende aus.

**Warum:** #E1 ist ein Platzhalter für das Ergebnis des Schritts namens #E1. Der Knoten tool ersetzt ihn durch dieses Ergebnis, bevor er forecast aufruft, also ist zwischen den Schritten kein Modellaufruf nötig. So kommt ReWOO mit zwei Modellaufrufen aus: einem zum Planen und einem im Knoten solve, der nach jedem Tool läuft, zu spät, um noch etwas auszufüllen.

### Frage 3 von 3

Der Lake Loop fällt aus, also liefert die Suche nach seinem Startort „Kein Ergebnis.“ Welches Muster kann die Vorhersage für den Lake Loop streichen, bevor sie läuft?

- **✓** Plan-and-execute: replan liest jedes Ergebnis vor dem nächsten Schritt.
- ReWOO: Der Knoten tool überspringt einen Schritt, dessen Eingabe nichts gefunden hat.
- LLMCompiler: Der Scheduler überspringt einen Task, dessen Eingabe nichts gefunden hat.
- Keines: Ein Plan kann sich nicht mehr ändern, sobald er geschrieben ist.

**Warum:** Plan-and-execute ruft replan nach jedem Schritt auf, also kann es die Vorhersage aus den übrigen Schritten streichen: Dafür bezahlen seine zusätzlichen Modellaufrufe. Der Knoten tool von ReWOO setzt nur Ergebnisse ein, also führt er forecast[#E2] mit „Kein Ergebnis.“ als Ort aus. Der Scheduler von LLMCompiler führt forecast($2) ebenfalls wie geplant aus; erst der Knoten join, nach allen Tasks, kann einen neuen Plan anfordern.

## Was du jetzt kannst

- Plan-and-execute, ReWOO und LLMCompiler als LangGraph.js-Graphen bauen.
- Zählen, was jedes Muster an Modellaufrufen, Supersteps und Zeit kostet, und sagen, woher der Unterschied kommt.
- Ein Muster wählen: neu planen, wenn Ergebnisse den Plan ändern können, einmal planen, wenn nicht, und unabhängige Schritte gleichzeitig ausführen.

---

[Zurück zu Agenten für Fortgeschrittene](README.md) · [Weiter: Rechenleistung einsetzen](08-spending-compute.md)
