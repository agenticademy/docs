# Agenten und Oberflächen: AG-UI und A2UI

Agentensysteme und Standards · Kapitel 3 von 8 · 25 Minuten

Die Arbeit eines Agenten mit AG-UI an eine App streamen und einen Agenten mit A2UI sichere Oberflächenkomponenten beschreiben lassen.

## Ziele

- Verfolge den AG-UI-Event-Stream eines Laufs bis zum Ende.
- Bestätige die Erstattung über ein Frontend-Tool.
- Schleus eine Komponente außerhalb des A2UI-Katalogs ein und sieh, wie sie abgelehnt wird.
- Besteh den Check.

## Der Text

### AG-UI: die Arbeit des Agenten als Stream

MCP verbindet einen Agenten mit Tools und A2A mit anderen Agenten. AG-UI (Agent–User Interaction Protocol) verbindet ihn mit der App, die ein Mensch nutzt. Die App sendet einen RunAgentInput (Thread-ID, Run-ID, Nachrichten, State und die Tools, die die App anbietet) und liest einen Stream von Events zurück, hier als Server-Sent Events.

Ein Lauf beginnt mit RUN\_STARTED und endet mit RUN\_FINISHED oder RUN\_ERROR. Text kommt als TEXT\_MESSAGE\_START, dann als Fragmente in TEXT\_MESSAGE\_CONTENT, dann TEXT\_MESSAGE\_END. Tool-Calls kommen genauso: TOOL\_CALL\_START, TOOL\_CALL\_ARGS, TOOL\_CALL\_END und TOOL\_CALL\_RESULT für Tools, die der Agent ausgeführt hat. Gemeinsamer State wandert als STATE\_SNAPSHOT, der ganze State, oder als STATE\_DELTA, eine Liste von JSON-Patch-Operationen. Der offizielle Client, @ag-ui/client 1.0.2, prüft Schema und Reihenfolge jedes Events, bevor er es anwendet.

### Frontend-Tools

Manche Tools gehören der App: ein Bestätigungsbutton, ein Datumswähler. Die App listet sie in RunAgentInput.tools. Ruft der Agent eines auf, endet der Lauf mit diesem Aufruf als offen; die App führt das Tool aus, hier indem sie den Kunden fragt, und startet den nächsten Lauf mit dem Ergebnis als Tool-Nachricht. Die Antwort des Menschen wird Teil der Unterhaltung, und der Agent entscheidet, was er damit macht.

### A2UI: Oberflächen als Daten

A2UI (Protokoll v0.9) lässt einen Agenten ein Stück Oberfläche beschreiben, ohne Code zu senden. createSurface öffnet eine Surface und nennt den Katalog, den sie nutzt; updateComponents sendet eine flache Liste von Komponenten, die per ID aufeinander verweisen; updateDataModel sendet die Werte, an die sie gebunden sind. Die App rendert nur Komponenten aus ihrem eigenen Katalog, hier sieben Typen: Card, Column, Row, List, Text, Button und Divider. Mit strikter Validierung lehnt @a2ui/web\_core 0.12.0 einen unbekannten Typ ab und die Surface bleibt, wie sie war, also kann ein Agent, oder ein Prompt, der ihn getäuscht hat, die App keinen neuen Code ausführen lassen.

### Was du hier machst

Ein Kunde fragt den Support-Chat nach der doppelten Abbuchung. Über AG-UI siehst du dir den Event-Stream an und bestätigst dann die Erstattung auf der Card, die die App zeichnet. Danach stellst du das Lab auf A2UI um, wo der Agent aus Nachrichten eine Erstattungs-Card baut. Schalte Abfangen ein, halte die Nachricht updateComponents an und füg eine Script-Komponente hinzu: web\_core lehnt sie ab.

## Probier es aus: Ein Agent baut einen Bildschirm

Vier Schritte mit je einer Aktion, dann zwei optionale. Das Lab unter den Schritten zeichnet jede Nachricht als Zeile, von oben nach unten. Solange ein A2UI-Lab offen ist, zeigt ein Baum unter dem Lab die Komponenten, die der Agent geschickt hat, über ihre id verbunden.

https://agenticademy.org/de/paths/agent-systems/03-agents-and-uis

## Check

### Frage 1 von 4

Was ist ein Frontend-Tool in AG-UI?

- **✓** Ein Tool, das die App ausführt, etwa ein Bestätigungsdialog; sein Ergebnis geht im nächsten Lauf an den Agenten zurück.
- Ein Tool, das auf dem Server Diagramme zeichnet.
- Eine Browser-Erweiterung, die der Agent installiert.

**Warum:** Der Agent fordert es wie jedes andere Tool an, aber die App deklariert und führt es aus, also kann ein Mensch an dem Schritt teilnehmen. Der Lauf endet mit dem offenen Aufruf; das Ergebnis startet den nächsten Lauf.

### Frage 2 von 4

Warum rendert A2UI aus einem Katalog, statt das Markup des Agenten auszuführen?

- Es ist schneller.
- **✓** Die App entscheidet, welche Komponenten es gibt, also kann ein Agent, oder ein Text, den er bekommen hat, die App keinen neuen Code ausführen lassen.
- HTML verlangt Kataloge.

**Warum:** UI als Daten zu beschreiben lässt die App die Kontrolle behalten. Mit strikter Validierung wird ein Komponententyp außerhalb des Katalogs abgelehnt, egal was der Agent sendet.

### Frage 3 von 4

Was trägt ein AG-UI-Event STATE\_DELTA?

- Eine vollständige Kopie des State.
- **✓** JSON-Patch-Operationen, die auf den gemeinsamen State angewendet werden.
- Den Token-Verbrauch des Modells.

**Warum:** Deltas halten Updates klein. Zielt ein Patch auf einen Pfad, den es nicht gibt, kann der Client ihn nicht anwenden und behält seinen vorigen State; ein STATE\_SNAPSHOT bringt ihn wieder in Einklang.

### Frage 4 von 4

Der Client empfängt TEXT\_MESSAGE\_CONTENT für eine messageId, die nie ein TEXT\_MESSAGE\_START hatte. Was passiert?

- Der Text wird trotzdem angezeigt.
- **✓** @ag-ui/client lehnt das Event wegen falscher Reihenfolge ab und stoppt den Lauf.
- Der Agent sendet es erneut.

**Warum:** Der Client prüft neben dem Schema auch die Reihenfolge der Events. Inhalt für eine Nachricht, die nie begonnen hat, ist ein Protokollfehler, also stoppt der Lauf.

## Was du jetzt kannst

- Einen AG-UI-Event-Stream lesen und sagen, was jedes Event bewirkt.
- Mit einem Frontend-Tool einen Menschen in den Lauf eines Agenten holen.
- Eine A2UI-Surface beschreiben: createSurface, updateComponents und updateDataModel.
- Erklären, wie der Katalog von A2UI eine App vor UI schützt, die ein Agent gebaut hat.

---

[Zurück zu Agentensysteme und Standards](README.md) · [Weiter: Agenten, die bezahlen: UCP und AP2](04-agent-commerce.md)
