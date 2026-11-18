# Ein Modell antwortet. Ein Agent handelt.

Agenten für alle · Kapitel 1 von 8 · 10 Minuten

Der Unterschied zwischen einem KI-Modell, das dir Text gibt, und einem Agenten, der Schritte unternimmt, um ein Ziel zu erreichen.

## Ziele

- Schick dieselbe Anfrage an ein Modell und an einen Agenten, und lass beide fertig werden.
- Finde den Schritt, in dem der Agent etwas außerhalb der Unterhaltung verändert hat.
- Besteh den Check.

## Der Text

### Ein Modell schreibt

Ein KI-Modell, wie das hinter einem Chatbot, ist ein Programm, das mit sehr viel Text trainiert wurde. Es bekommt einige Wörter und sagt die Wörter voraus, die als Nächstes kommen sollten. Das reicht, um eine Frage zu beantworten, einen Brief zusammenzufassen oder eine E-Mail zu entwerfen.

Modelle werden von Firmen und Forschungsgruppen gebaut. Manche Modelle sind offen: Alle können sie herunterladen und ausführen. Viele werden auf Hugging Face geteilt, einer Website zum Teilen von Modellen, und manche, etwa die Nemotron-Familie von NVIDIA, kannst du kostenlos ausprobieren, mit Grenzen.

Für sich allein kann ein Modell dir nur Text zurückgeben. Es kann nicht in deinen Kalender schauen und nichts buchen. Was es auch vorschlägt, du musst es noch selbst erledigen.

### Ein Agent handelt

Ein Agent ist ein Programm, das um ein Modell herum gebaut ist. Du gibst ihm ein Ziel, und er kann Tools benutzen: andere Programme, die etwas nachschlagen oder etwas verändern, etwa einen Kalender, eine Karte oder eine Buchungsseite.

Der Agent arbeitet in Schritten. Das Modell entscheidet, was als Nächstes passiert, der Agent benutzt das Tool, und das Modell liest, was zurückkam. Das wiederholt sich, bis das Ziel erreicht ist oder bis der Agent dich braucht. „Agentische KI“ ist der allgemeine Name für Systeme, die so arbeiten.

### Dieselbe Anfrage, zwei Ergebnisse

Bitte ein Modell, einen Tisch für sechs am Samstag zu reservieren, und es erklärt dir, wie du einen reservierst. Bitte einen Agenten darum, und er kann deinen Kalender prüfen, ein Restaurant finden, dich um Bestätigung bitten und die Reservierung vornehmen. Die Simulation unten spielt beides durch, damit du vergleichen kannst.

### Warum der Unterschied zählt

Text kannst du leicht ignorieren. Handlungen nicht: Eine Reservierung, eine Zahlung oder eine gesendete E-Mail hat Folgen in der Welt. Deshalb fragt ein guter Agent nach, bevor er etwas Wichtiges tut, und deshalb widmet dieser Lernpfad der Sicherheit ein ganzes Kapitel.

### Vier Wörter für diesen Lernpfad

Modell: das Programm, das Text liest und schreibt. Tool: etwas, das ein Agent benutzen kann, um etwas nachzuschlagen oder zu handeln. Ziel: das, worum du den Agenten gebeten hast. Schritt: eine Entscheidung des Modells und eine Handlung des Agenten.

## Probier es aus: Eine Anfrage, zwei Ergebnisse

Stell dieselbe Anfrage erst einem Modell, dann einem Agenten. Antworte dem Agenten, wenn er fragt, und finde dann den Schritt, der etwas in der Welt verändert hat.

https://agenticademy.org/de/paths/agents-for-everyone/01-model-or-agent

## Check

### Frage 1 von 4

Du bittest einen Chatbot um ein Rezept, und er schreibt eines auf. Ist da ein Agent am Werk?

- Ja, weil er KI benutzt hat.
- **✓** Nein. Er hat Text zurückgegeben, und nichts wurde für dich erledigt.
- Ja, weil er verstanden hat, was du meinst.

**Warum:** Ein Rezept zu schreiben ist etwas, das ein Modell allein kann. Ein Agent würde Tools benutzen, um zu handeln, zum Beispiel prüfen, was in deiner Online-Lebensmittelbestellung ist, und die fehlenden Zutaten hinzufügen.

### Frage 2 von 4

Was hat ein Agent, das ein Modell allein nicht hat?

- Ein größeres Gedächtnis des Internets.
- **✓** Tools, die er benutzen kann, und eine Schleife von Schritten, die weiterläuft, bis das Ziel erreicht ist.
- Eine Stimme und ein Gesicht.

**Warum:** Ein Agent ist ein Modell plus Tools plus eine Schleife: entscheiden, handeln, das Ergebnis lesen, wieder entscheiden. Das Modell darin kann genau dasselbe sein, das ein Chatbot benutzt.

### Frage 3 von 4

Warum hat dich der Agent in der Simulation vor dem Reservieren gefragt?

- **✓** Eine Reservierung hat echte Folgen, also sollte ein Mensch sie zuerst bestätigen.
- Er wusste nicht, wie man reserviert.
- Agenten fragen vor jedem einzelnen Schritt.

**Warum:** Etwas nachzuschlagen ist harmlos, eine Reservierung nicht. Gute Agenten fragen vor Handlungen, die schwer rückgängig zu machen sind oder dich zu etwas verpflichten, und erledigen den Rest selbst.

### Frage 4 von 4

Was davon ist ein Tool im Sinne dieses Kapitels?

- Der Text, mit dem das Modell trainiert wurde.
- Deine Anfrage.
- **✓** Ein Kalender, den der Agent lesen kann.

**Warum:** Ein Tool ist etwas, das der Agent während seiner Schritte benutzen kann, um etwas nachzuschlagen oder zu handeln. Deine Anfrage ist das Ziel, und der Trainingstext ist das, woraus das Modell gelernt hat.

## Was du jetzt kannst

- Ein Modell von einem Agenten mit einer Frage unterscheiden: Kann es handeln oder nur antworten?
- Die Teile eines Agenten nennen: ein Modell, Tools, ein Ziel und eine Schleife von Schritten.
- Erklären, warum ein Agent dich vor einer wichtigen Handlung fragen sollte.

---

[Zurück zu Agenten für alle](README.md) · [Weiter: Die Schleife: schauen, entscheiden, handeln](02-the-agent-loop.md)
