# Observability: Traces und Läufe

Agentensysteme und Standards · Kapitel 7 von 8 · 20 Minuten

Jeden Modellaufruf, Tool-Call und Schritt als Baum von Läufen aufzeichnen und ihn lesen, um zu finden, was langsam oder kaputt ist.

## Ziele

- Finde den langsamsten Modell- oder Tool-Lauf im Trace.
- Finde den fehlgeschlagenen Tool-Lauf und seinen Elternlauf.
- Exportiere die Läufe im Format von LangSmith.
- Besteh den Check.

## Der Text

### Läufe und Traces

Hinter der Antwort eines Agenten stecken viele Schritte. Tracing zeichnet jeden davon als Lauf auf: einen Graphen, einen Knoten, einen Modellaufruf oder einen Tool-Call, mit Inputs, Outputs, Start- und Endzeit und einem eventuellen Fehler. Die Läufe hinter einer Anfrage bilden einen Baum, den Trace.

LangChain.js und LangGraph.js tracen über Callbacks. Gib einen Tracer wie LangChainTracer in den callbacks der Config mit, und jedes Runnable meldet ihm seinen Start und sein Ende.

### Das Lauf-Format von LangSmith

Jeder Lauf hat einen run\_type, etwa chain, llm oder tool, eine id, eine trace\_id, die der ganze Baum teilt, und eine parent\_run\_id. Seine dotted\_order ist ein Schlüssel aus Startzeit und ID jedes Laufs von der Wurzel bis zu ihm, sodass eine Sortierung danach den Baum in der Reihenfolge des Geschehens auslegt. Modell-Läufe tragen usage\_metadata mit Token-Zahlen. LangGraph fügt den Metadaten thread\_id, langgraph\_node und langgraph\_step hinzu.

### Supersteps und der Baum der Läufe

LangGraph führt einen Graphen in Supersteps aus. In jedem Superstep laufen die fälligen Knoten, und ihre Updates werden gemeinsam angewendet, bevor der nächste Superstep beginnt. langgraph\_step ist die Nummer dieses Supersteps, also lässt sich ein Trace auf zwei Arten lesen: als Baum der Läufe, jeder Lauf unter seinem Elternlauf, und als Superstep-Zeitleiste, eine Gruppe von Läufen pro Schritt. Der Wasserfall im Lab gruppiert seine Zeilen nach Superstep.

### Worauf du achten solltest

Latenz: welcher Lauf lang ist, und ob es das Modell oder ein Tool ist. Fehler: ein fehlgeschlagener Tool-Lauf und der Knoten, der ihn aufgerufen hat. Tokens: welche Aufrufe am meisten lesen. Schleifen: derselbe Schritt immer wieder.

Traces enthalten Prompts, Tool-Ergebnisse und Kundendaten, also behandle sie als sensibel. Das LangSmith-SDK kann vor dem Senden einen Anonymizer über jeden Lauf laufen lassen. OpenTelemetry ist der allgemeine Standard für Traces über Dienste hinweg; LangSmith nimmt auch OpenTelemetry-Traces an.

## Probier es aus: Der Lauf-Wasserfall

Fünf Schritte: Führ den Agenten aus und sieh zu, wie sein Trace wächst, lies seine Supersteps, finde den langsamsten Lauf und das fehlgeschlagene Tool und exportiere dann die Läufe.

https://agenticademy.org/de/paths/agent-systems/07-observability

## Check

### Frage 1 von 5

Was gibt dir dotted\_order?

- Die Kosten eines Laufs.
- **✓** Einen sortierbaren Schlüssel, der jeden Lauf in den Baum seines Trace einordnet, in zeitlicher Reihenfolge.
- Den Namen des Modells.

**Warum:** Er wird aus Startzeit und ID jedes Laufs von der Wurzel bis zu diesem gebildet, sodass eine Sortierung danach den Baum in der Reihenfolge des Geschehens wieder aufbaut, ohne Eltern-Links einzeln zu folgen.

### Frage 2 von 5

Zwei Läufe in einem Trace haben denselben langgraph\_step. Was sagt dir das?

- Sie wurden vom selben Tool aufgerufen.
- **✓** Sie liefen im selben Superstep des Graphen.
- Sie haben gleich lang gedauert.

**Warum:** langgraph\_step ist die Nummer des Supersteps. In jedem Superstep führt LangGraph die fälligen Knoten aus und wendet ihre Updates gemeinsam an; jeder Lauf in diesen Knoten trägt diese Nummer.

### Frage 3 von 5

Was ist ein Trace?

- Eine Logdatei mit Fehlern.
- **✓** Der Baum der Läufe, Modell, Tool und Chain, hinter einer Anfrage.
- Ein Screenshot der App.

**Warum:** Eine Anfrage wird zu einem Trace; jeder Schritt darin ist ein Lauf mit seinem Elternlauf.

### Frage 4 von 5

Ein Tool-Lauf ist fehlgeschlagen, aber der Agent hat trotzdem geantwortet. Warum ist der Elternlauf nicht als fehlgeschlagen markiert?

- Der Tracer versteckt Fehler.
- **✓** Der tools-Knoten hat den Fehler abgefangen und als Ergebnis an das Modell zurückgegeben, also ist der Knoten selbst beendet.
- Elternläufe schlagen nie fehl.

**Warum:** Ein Fehler wird an dem Lauf vermerkt, der ihn geworfen hat. Hier hat der Knoten ihn in eine Fehler-ToolMessage verwandelt, das Modell hat sie gelesen und es erneut versucht.

### Frage 5 von 5

Was solltest du nicht an einen geteilten Tracing-Dienst senden?

- Die Zeiten der Läufe.
- **✓** Persönliche Daten von Kunden in Prompts und Tool-Ergebnissen, außer du brauchst sie und darfst das.
- Lauf-Typen.

**Warum:** Traces zeichnen Inputs und Outputs vollständig auf. Schwärze oder begrenze, was erfasst wird, bevor es dein System verlässt, zum Beispiel mit dem Anonymizer des SDK.

## Was du jetzt kannst

- Eine Superstep-Zeitleiste und einen Baum der Läufe lesen.
- Den langsamen oder fehlgeschlagenen Lauf in einem Trace finden, und den Lauf, der ihn aufgerufen hat.
- Die wichtigsten Felder des Lauf-Formats von LangSmith nennen.
- Persönliche Daten schwärzen, bevor ein Trace dein System verlässt.

---

[Zurück zu Agentensysteme und Standards](README.md) · [Weiter: Evaluation und Sicherheit](08-evaluation-and-safety.md)
