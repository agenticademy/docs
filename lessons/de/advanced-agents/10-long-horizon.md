# Agenten mit langem Horizont

Agenten für Fortgeschrittene · Kapitel 10 von 10 · 25 Minuten

Ein Job, der sich über mehrere Sessions erstreckt, braucht Compaction innerhalb einer Session, Übergabedateien und Memory zwischen den Sessions und eine Prüfung seiner ganzen Trajektorie.

## Ziele

- Führe Session 1 aus und lies die Dateien, die sie für die nächste Session hinterlässt.
- Starte Session 2 nur aus den Dateien und sieh zu, wie Compaction ältere Nachrichten durch eine Zusammenfassung ersetzt.
- Bewerte die Tool-Aufrufe der Coding-Sessions und öffne eine Regel, die Session 3 gebrochen hat.
- Bestehe die Prüfung.

## Der Text

### Länger als ein Kontext

Ein Job mit langem Horizont braucht mehr Modellaufrufe, als ein Kontextfenster fasst, oder mehr als eine Session: etwa eine App Feature für Feature bauen. Das Kontextfenster ist das Höchste, was ein Modell in einem Aufruf liest, gezählt in Tokens. Eine Session ist hier ein Lauf des Agenten auf einem neuen Thread. Zwei Dinge halten so einen Job auf Kurs: den Kontext innerhalb einer Session verkleinern und die Arbeit zwischen Sessions übergeben.

### Auslagern und Compaction

Der Beitrag von LangChain zum Kontextmanagement in Deep Agents (2026-01-28) beschreibt drei Ebenen. Ein Tool-Ergebnis über 20.000 Tokens kommt in eine Datei, und der Agent bekommt ihren Pfad und eine Vorschau. Ab 85 % des Fensters werden die Eingaben alter Datei-Schreib- und Bearbeitungsaufrufe zu Verweisen auf ihre Dateien. Reicht das nicht, kommt Compaction: Ein Modell schreibt eine Zusammenfassung der Absicht, der Artefakte und der nächsten Schritte, die Zusammenfassung ersetzt die älteren Nachrichten, und das vollständige Transkript bleibt auf der Festplatte. Der nachgebaute Harness dieses Labs lagert ab 20.000 Tokens aus und kompaktiert bei 85 % des Fensters (summarize: { window, fraction, keep }), ohne mittlere Ebene.

### Übergabedateien und Memory

Der Beitrag von Anthropic zu Harnesses für lang laufende Agenten (2025-11-26) beginnt jeden Job mit einer Initialisierungs-Session. Sie schreibt eine Feature-Liste als JSON, in der jedes Feature fehlschlägt, eine Fortschrittsdatei, ein init.sh-Skript und einen ersten Git-Commit. Jede spätere Session liest die Fortschrittsdatei, das Git-Log und die Feature-Liste, führt einen einfachen End-to-End-Test aus, arbeitet an einem Feature, setzt dessen Feld passes erst nach dem Testen auf true und endet mit einem Commit und einer Fortschrittsnotiz. Sonst ändert sich nichts an der Liste. Für Fakten, die jeder Thread braucht, hält der Store von LangGraph Langzeit-Memory unter einem Namespace, außerhalb jedes Threads.

### Reset oder Compaction

Eine Session aus diesen Dateien zu starten, ist ein Kontext-Reset: ein sauberer Kontext und eine schriftliche Übergabe. Compaction behält einen Kontext und verkleinert ihn. Im Beitrag von Anthropic vom 2026-03-24 schloss Sonnet 4.5 frühzeitig ab, als sich sein Kontext füllte, also setzte dieser Harness zurück; mit Opus 4.5 wechselte er zu Compaction.

### Die Trajektorie bewerten

Eine Trajektorie ist die Folge der Tool-Aufrufe, die ein Lauf gemacht hat. Eine Antwort kann „fertig“ sagen, obwohl kein Test lief, also bewerte die Aufrufe mit Regeln im Code, wie die Code-Checks in Agentensysteme, Kapitel 8. Lies die Aufrufe aus dem Trace: Nach der Compaction enthalten die Nachrichten die frühen nicht mehr.

## Probier es aus: Drei Sessions, ein Job

Führe Session 1 aus, dann Session 2 nur aus ihren Dateien, dann Session 3, und bewerte die Trajektorien der Coding-Sessions.

https://agenticademy.org/de/paths/advanced-agents/10-long-horizon

## Check

Eine Coding-Session eines langen Jobs startet auf einem neuen Thread. Bring ihre Schritte in die Reihenfolge, in der der Harness aus dem Beitrag von Anthropic sie ausführt.

1. progress.txt, das Git-Log und feature\_list.json lesen.
2. Den Smoke-Test ausführen, um zu prüfen, dass die App noch läuft.
3. Den Code für ein Feature schreiben.
4. Die Tests dieses Features ausführen.
5. passes dieses Features in feature\_list.json auf true setzen.
6. Eine Zeile in progress.txt ergänzen und committen.

**Warum:** Der neue Thread weiß nur, was in den Dateien steht, also liest er sie zuerst. Der Smoke-Test fängt alles ab, was die letzte Session kaputt hinterlassen hat, bevor neuer Code darauf kommt. Ein Feature besteht erst, wenn seine eigenen Tests auf dem neuen Code bestehen: passes zuerst zu setzen, ist die Abkürzung, die der Checker in Session 3 markiert hat. Die Fortschrittszeile und der Commit sind das, was die nächste Session liest.

## Was du jetzt kannst

- Einen langen Job mit Feature-Liste, Fortschrittsdatei und Commits einrichten, sodass jede Session aus den Dateien starten kann.
- Einen Kontext mit summarize ab einem Schwellenwert kompaktieren und große Tool-Ergebnisse in Dateien auslagern.
- Langzeit-Memory in einem LangGraph-Store halten, den jeder Thread liest.
- Einen Lauf nach seinen Tool-Aufrufen bewerten, mit Regeln im Code, die aus dem Trace lesen.

---

[Zurück zu Agenten für Fortgeschrittene](README.md)
