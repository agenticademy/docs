# Tools: Wie ein Agent die Welt erreicht

Agenten für alle · Kapitel 3 von 8 · 12 Minuten

Mit Tools schlägt ein Agent etwas nach und erledigt Dinge, und das Modell kennt jedes Tool nur über sein Etikett.

## Ziele

- Gib dem Agenten für drei Anfragen das richtige Tool.
- Sieh, wie eine vage Tool-Beschreibung den Agenten zum falschen Tool führt.
- Besteh den Check.

## Der Text

### Was ein Tool ist

Ein Tool ist ein Programm, mit dem ein Agent etwas tun kann, was er mit Text allein nicht schafft: das Wetter prüfen, einen Kalender lesen, etwas ausrechnen, eine E-Mail senden. Um eines zu benutzen, füllt der Agent die Angaben aus, die das Tool braucht (für eine Wettervorhersage eine Stadt und einen Tag), und liest, was zurückkommt.

### Tools, die lesen, und Tools, die etwas verändern

Manche Tools lesen nur: Sie schlagen etwas nach und verändern nichts. Andere schreiben: Sie verändern etwas in der Welt, etwa indem sie eine E-Mail senden oder ein Ticket kaufen. Ein Fehler mit einem lesenden Tool kostet eine Runde. Ein Fehler mit einem schreibenden Tool kann viel mehr kosten, und deshalb fragen umsichtige Agenten dich, bevor sie eines benutzen.

### Das Etikett ist alles, was das Modell sieht

Das Modell sieht nie in ein Tool hinein. Für jedes Tool bekommt es ein Etikett: einen Namen, eine kurze Beschreibung dessen, was das Tool tut, und die Angaben, die es braucht. Es wählt, indem es deine Anfrage mit diesen Etiketten vergleicht.

Die Worte auf dem Etikett zählen also genauso viel wie das Tool selbst. Ein klares Etikett führt zur richtigen Wahl. Ein vages führt zu Fehlern, selbst wenn das richtige Tool direkt daneben liegt.

### Weniger Tools, weniger Fehler

Gib einem Agenten nur die Tools, die eine Aufgabe braucht. Weniger Auswahl heißt weniger Fehlgriffe, und ein Agent kann kein Tool missbrauchen, das er nicht hat.

Unten spielst du die Hilfe des Agenten: Gib ihm für drei Anfragen das richtige Tool. Dann änderst du ein Etikett und siehst, was der Agent damit macht.

## Probier es aus: Wähl das richtige Tool

Gib dem Agenten für drei Anfragen das richtige Tool; wähl ein Tool, um sein Etikett zu lesen. Lass dann den Agenten anhand der Etiketten wählen, mach das Etikett von E-Mail vage und schick die Anfrage noch einmal.

https://agenticademy.org/de/paths/agents-for-everyone/03-tools

## Check

### Frage 1 von 3

Was weiß das Modell über ein Tool, wenn es entscheidet, es zu benutzen?

- Wie der Code des Tools innen funktioniert.
- **✓** Seinen Namen, seine Beschreibung und die Angaben, die es braucht.
- Alles, was das Tool je zurückgegeben hat.

**Warum:** Das Etikett ist alles, was das Modell bekommt. Deshalb zählt eine klare Beschreibung genauso viel wie das Tool selbst.

### Frage 2 von 3

Welches davon ist ein schreibendes Tool, also eines, das etwas verändert?

- Das Wetter von morgen prüfen.
- Ein Restaurant suchen.
- **✓** Eine E-Mail senden.

**Warum:** Eine E-Mail zu senden verändert etwas außerhalb der Unterhaltung und lässt sich nicht rückgängig machen. Wetter oder Restaurants nachzuschlagen liest nur.

### Frage 3 von 3

Warum gibst du einem Agenten nur die Tools, die eine Aufgabe braucht?

- Es macht den Agenten langsamer, aber günstiger.
- **✓** Weniger Auswahl heißt weniger Fehlgriffe, und weniger Schaden, wenn doch einer passiert.
- Agenten können nur drei Tools halten.

**Warum:** Ein Agent kann kein Tool missbrauchen, das er nicht hat. Wenige, klar beschriftete Tools machen gute Entscheidungen leichter und schlechte weniger teuer.

## Was du jetzt kannst

- Erklären, was ein Tool ist, und lesende von schreibenden Tools unterscheiden.
- Erklären, warum die Beschreibung eines Tools die Entscheidungen des Agenten prägt.
- Begründen, warum ein Agent nur die Tools bekommen sollte, die er braucht.

---

[Zurück zu Agenten für alle](README.md) · [Weiter: Gedächtnis und Kontext](04-memory-and-context.md)
