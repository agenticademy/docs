# Gedächtnis und Kontext

Agenten für alle · Kapitel 4 von 8 · 12 Minuten

Was ein Agent bei jedem Schritt sehen kann, ist begrenzt. Was er behält, verwirft und aufschreibt, bestimmt also, was er weiß.

## Ziele

- Füll den Kontext, bis die frühesten Nachrichten herausfallen.
- Speichere die richtige Information, damit der Agent später richtig antwortet.
- Besteh den Check.

## Der Text

### Was das Modell liest

Jedes Mal, wenn das Modell einen Schritt entscheidet, liest es einen Textblock, den Kontext: deine Anfrage, die bisherige Unterhaltung und was die Tools zurückgegeben haben. Mehr hat es nicht. Steht etwas nicht im Kontext, weiß das Modell es für diesen Schritt nicht.

### Ein Schreibtisch mit wenig Platz

Der Kontext hat eine Größengrenze, gezählt in Token. Ein Token ist ein Stück eines Worts: Ein kurzes Wort ist etwa ein Token, und im Englischen ist ein Token etwa vier Zeichen lang. Die Grenzen unterscheiden sich je nach Modell, von ein paar Tausend Token bis zu einer Million oder mehr, aber es gibt immer eine.

Stell dir den Kontext als Schreibtisch vor. Nur so viel Papier passt gleichzeitig darauf.

### Wenn der Schreibtisch voll ist

Eine lange Unterhaltung füllt den Schreibtisch. Dann muss etwas weg: Die ältesten Nachrichten werden verworfen, oder mehrere werden durch eine kurze Zusammenfassung ersetzt. Was verworfen wird, weiß der Agent nicht mehr, auch wenn du es ihm früher gesagt hast.

### Gedächtnis: ein Notizbuch neben dem Schreibtisch

Gedächtnis ist etwas anderes. Es sind Notizen, die der Agent irgendwo außerhalb des Kontexts schreibt, etwa in eine Datei, und wieder liest, wenn sie gebraucht werden. Eine Notiz übersteht das Aufräumen des Schreibtischs, aber nur, wenn der Agent sie gespeichert hat, und nur der Teil, den er aufschreiben wollte.

### Mehr ist nicht immer besser

Ein voller Kontext braucht länger zum Lesen, kostet mehr und kann die eine Information begraben, auf die es ankommt. Gute Agenten lassen auf dem Schreibtisch, was der nächste Schritt braucht, und schreiben den Rest ins Notizbuch.

## Probier es aus: Ein Schreibtisch mit wenig Platz

Füll den Schreibtisch, bis die ältesten Nachrichten herunterfallen, und sieh, was der Agent vergisst. Fang dann neu an, speichere eine Information im Notizbuch und frag noch einmal.

https://agenticademy.org/de/paths/agents-for-everyone/04-memory-and-context

## Check

### Frage 1 von 3

Was steht für einen Schritt im Kontext eines Agenten?

- Alles im Internet.
- **✓** Die Anfrage, die bisherige Unterhaltung und Tool-Ergebnisse, die noch hineinpassen.
- Nur deine letzte Nachricht.

**Warum:** Das Modell liest nur, was ihm für diesen Schritt vorgelegt wird, bis zu seiner Größengrenze.

### Frage 2 von 3

Der Kontext ist voll, und eine neue Nachricht kommt an. Was passiert meistens?

- Der Agent hört endgültig auf zu arbeiten.
- **✓** Ältere Nachrichten werden verworfen oder zusammengefasst, um Platz zu schaffen.
- Das Modell bekommt einen größeren Kontext.

**Warum:** Die Grenze ist für ein Modell fest, also muss der Agent Platz schaffen. Was er verwirft, weiß er nicht mehr, außer er hat eine Notiz gespeichert.

### Frage 3 von 3

Wie unterscheidet sich Gedächtnis vom Kontext?

- **✓** Gedächtnis sind Notizen, die außerhalb des Kontexts liegen und bei Bedarf wieder gelesen werden.
- Das ist dasselbe.
- Gedächtnis ist das, was das Modell im Training gelernt hat.

**Warum:** Gedächtnis übersteht das Leeren des Kontexts, weil es woanders liegt. Der Agent muss sich entscheiden, es zu speichern und wieder zu lesen.

## Was du jetzt kannst

- Erklären, was ein Kontext ist und warum er eine Grenze hat.
- Sagen, was passiert, wenn er voll wird.
- Kontext von Gedächtnis unterscheiden und sagen, warum es zählt, die richtigen Informationen zu speichern.

---

[Zurück zu Agenten für alle](README.md) · [Weiter: Pläne und Agenten im Team](05-plans-and-teams.md)
