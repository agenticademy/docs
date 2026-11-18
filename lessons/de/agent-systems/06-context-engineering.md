# Context Engineering und Retrieval

Agentensysteme und Standards · Kapitel 6 von 8 · 25 Minuten

Auswählen, was jeder Modellaufruf liest: grep oder Vektorsuche, große Ergebnisse auslagern, ein erster Blick auf Subagenten und ein Gedächtnis, das über Threads hinweg hält.

## Ziele

- Führ grep und Vektorsuche für dieselben Wörter aus und vergleich.
- Lass ein großes Tool-Ergebnis auslagern, statt den Kontext zu füllen.
- Gib eine Suche an Subagenten ab und vergleich die Kontextgrößen.
- Besteh den Check.

## Der Text

### Was jeder Aufruf liest

Ein Modellaufruf liest seinen Kontext: den System-Prompt, die bisherigen Nachrichten und jedes Tool-Ergebnis. Context Engineering heißt entscheiden, was hineinkommt. Zu wenig, und der Agent rät; zu viel, und es kostet mehr, läuft langsamer und lenkt ihn mit Text ab, der keine Rolle spielt oder der eingeschleust wurde.

### grep oder Vektoren

grep findet exakten Text, Zeile für Zeile: eine Bestellnummer, eine Ticket-ID, ein Datum. Es kostet nichts und übersieht alles, was anders formuliert ist.

Vektorsuche findet ähnliche Bedeutung. Jeder Chunk jeder Datei wird in einen Vektor umgewandelt (hier 384 Zahlen aus dem Embedding-Modell all-MiniLM-L6-v2), und die Chunks werden nach Kosinus-Ähnlichkeit zum Vektor der Anfrage sortiert. Sie findet „payment taken two times“ für „charged twice“, stuft bei 48213 aber Tickets zu den Bestellungen 48144 und 48231 vor dem richtigen ein. Filter wie ein Ordner oder ein Datum halten veraltete oder fremde Chunks draußen.

### Auslagern, Skills und Subagenten

Ein Tool kann weit mehr zurückgeben, als ein Kontext fassen kann. Eviction speichert ein großes Ergebnis in eine Datei und gibt dem Modell den Pfad und eine kurze Vorschau; der Agent durchsucht die Datei dann mit grep und liest nur die Zeilen, die er braucht.

Skills folgen derselben Idee: Der Agent sieht den Namen jedes Skills und eine einzeilige Beschreibung und liest die vollständigen Anweisungen (eine SKILL.md-Datei) erst, wenn eine Aufgabe sie braucht.

Ein Subagent bekommt einen frischen Kontext mit nur seiner Aufgabe und gibt einen Bericht zurück, sodass sein Lesen außerhalb des Hauptkontexts bleibt. Agenten für Fortgeschrittene, Kapitel 9 und 10, behandelt Subagenten, Schwellenwerte und Compaction ausführlich.

### Gedächtnis über Threads hinweg

Ein Thread ist eine Unterhaltung: Sein Checkpointer hält die Nachrichten dieses Threads, und ein neuer Thread beginnt leer. Langzeitgedächtnis ist ein Store, den alle Threads teilen. Der InMemoryStore von LangGraph legt Einträge unter einem Namespace ab, etwa customers / mara-lind; ein Thread legt dort eine Notiz ab, und ein späterer Thread sucht danach. In Produktion ist der Store eine Datenbank.

### In diesem Lab

Haldens Support-Agent bearbeitet das Ticket T-48213. Sein Harness baut Deep Agents (ein Open-Source-Harness von LangChain) auf LangGraph.js nach: Dateisystem-Tools, Eviction ab 20.000 Tokens und task-Subagenten. Der Gedächtnis-Schritt nutzt den InMemoryStore von LangGraph.js selbst.

## Probier es aus: Was der Agent liest

Such dieselben Wörter mit grep und mit Vektoren, lass den Zahlungsexport auslagern, gib das Lesen an Subagenten ab und speichere dann eine Notiz, die ein späterer Thread wieder liest.

https://agenticademy.org/de/paths/agent-systems/06-context-engineering

## Check

### Frage 1 von 4

Der Agent muss jedes Ticket finden, das die Bestellung 48213 erwähnt. Welche Suche passt am besten?

- Vektorsuche.
- **✓** grep nach 48213.
- Das Modell bitten, es sich zu merken.

**Warum:** Eine exakte ID ist genau das, wofür grep da ist. Vektorsuche sortiert nach Bedeutung, und in diesem Lab stuft sie Tickets zu den Bestellungen 48144 und 48231 vor dem richtigen ein.

### Frage 2 von 4

Ein Tool gibt einen Export mit 40.000 Zeilen zurück. Was legt Eviction in den Kontext?

- Alle 40.000 Zeilen.
- **✓** Den Pfad der Datei und eine kurze Vorschau; der Agent liest bei Bedarf Seiten daraus.
- Gar nichts.

**Warum:** Das volle Ergebnis wird in einer Datei gespeichert, sodass der Kontext klein bleibt und der Agent die Daten trotzdem mit grep durchsuchen und seitenweise lesen kann.

### Frage 3 von 4

Was kommt von einem Subagenten zu dem Agenten zurück, der ihn gestartet hat?

- Seine ganze Unterhaltung.
- **✓** Ein abschließender Bericht über seine Arbeit.
- Seine Tools.

**Warum:** Was der Subagent liest, bleibt in seinem eigenen Kontext. Nur sein Bericht kommt in den Kontext des Elternagenten, was diesen klein hält und eingeschleusten Text draußen.

### Frage 4 von 4

Thread 1 hat eine Notiz im Store gespeichert. Thread 2 beginnt ohne Nachrichten. Kann er die Notiz lesen?

- Nein: Ein neuer Thread beginnt leer.
- **✓** Ja: Alle Threads teilen den Store; nur die Nachrichten gehören zu einem Thread.
- Nur wenn Thread 2 die Nachrichten von Thread 1 kopiert.

**Warum:** Der Checkpointer hält die Nachrichten jedes Threads getrennt. Der Store liegt außerhalb der Threads, also kann jeder Thread den Namespace der Kundin durchsuchen.

## Was du jetzt kannst

- Für eine Anfrage zwischen grep und Vektorsuche wählen und sagen, wann ein Ordner- oder Datumsfilter hilft.
- Große Tool-Ergebnisse auslagern und seitenweise lesen.
- Subagenten nutzen, um den Kontext eines Hauptagenten klein und sauber zu halten.
- Eine Notiz in einem Langzeit-Store speichern und in einem späteren Thread wieder lesen.

---

[Zurück zu Agentensysteme und Standards](README.md) · [Weiter: Observability: Traces und Läufe](07-observability.md)
