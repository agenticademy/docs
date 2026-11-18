# Was KI ist und was nicht

KI-Grundlagen · Kapitel 1 von 8 · 12 Minuten

Die meisten KI-Systeme, die heute im Einsatz sind, lernen Muster aus Beispielen und nutzen sie, um für etwas Neues ein Ergebnis abzuleiten. Ein solches System ist kein Verstand, keine Datenbank und keine Suchmaschine, und seine Beispiele prägen jede Antwort.

## Ziele

- Platziere Beispiele zweier Arten und lass den Klassifikator sie lernen.
- Teste eine neue Frucht und sieh, welche Beispiele entscheiden.
- Füg eine einseitige Lieferung Beispiele hinzu und sieh zu, wie sich die Grenze verschiebt.
- Besteh den Check.

## Der Text

### Aus Beispielen lernen

Die meisten KI-Systeme, die heute im Einsatz sind, sind Programme, die Muster aus Beispielen lernen und diese Muster dann nutzen, um für etwas Neues ein Ergebnis zu erzeugen. Die Beispiele heißen Daten. Das Ergebnis kann ein Label wie „Spam“ sein, eine Zahl wie die Verkaufszahlen der nächsten Woche oder das nächste Wort eines Satzes.

Ein Ergebnis auf diese Weise zu ermitteln, heißt Inferenz: Die Antwort kommt aus Mustern, nicht aus dem Nachschlagen.

### Erst Training, dann Nutzung

Ein solches System hat zwei Phasen. Im Training bekommt es viele Beispiele gezeigt und stellt sich so ein, dass es zu ihnen passt. In der Nutzung wendet es das Gelernte auf Eingaben an, die es nie gesehen hat. Die Nutzung ändert nicht, was es gelernt hat; dafür braucht es eine weitere Trainingsrunde.

### Was es nicht ist

Es ist kein Verstand: Es berechnet Ergebnisse aus Mustern und will nichts. Es ist keine Datenbank: Eine Datenbank gibt nur zurück, was gespeichert wurde, während ein KI-System auch für Eingaben antwortet, die niemand gespeichert hat, und sich irren kann. Es ist keine Suchmaschine: Es ruft keine Seiten ab, auch wenn manche Tools es mit einer verbinden.

### Die Beispiele entscheiden

Ein System lernt nur aus seinen Beispielen, also prägen sie jede Antwort. Wenn die Beispiele einseitig sind, also eine Art oder eine Quelle viel häufiger vorkommt als der Rest, neigen die Antworten in dieselbe Richtung. Ein System, das vor allem aus Fotos aus einem einzigen Land gelernt hat, schneidet bei Fotos von anderswo meist schlechter ab.

### In der Visualisierung

Du trainierst einen Klassifikator, ein Programm, das Dinge in Arten einteilt, damit er Zitronen anhand von Größe und Farbe von Orangen unterscheidet. Jede Frucht ist ein Punkt auf einer Karte. Um eine neue Frucht zu beurteilen, sucht er die 3 Beispiele, die ihr auf der Karte am nächsten liegen, ihre nächsten Nachbarn, und antwortet mit der Art, zu der die meisten von ihnen gehören. Die Linie, an der seine Antwort von einer Art zur anderen wechselt, ist seine Grenze.

Bei diesem einfachen Klassifikator heißt Training, die Beispiele zu behalten. Große Modelle passen dagegen beim Training eine riesige Zahl interner Werte an und behalten diese Werte statt der Beispiele.

## Probier es aus: Bring einem winzigen Klassifikator etwas bei

Platziere Zitronen und Orangen auf der Karte, teste eine Frucht, die er nicht gesehen hat, füg dann eine einseitige Lieferung hinzu und sieh zu, wie sich die Grenze verschiebt.

https://agenticademy.org/de/paths/ai-principles/01-what-ai-is

## Check

### Frage 1 von 3

Ein System wurde mit Tausenden Fotos trainiert, die mit „Katze“ oder „Hund“ beschriftet sind. Jetzt beschriftet es ein Foto, das es nie gesehen hat. Was tut es?

- Es schlägt das Foto in einem Speicher voller Antworten nach.
- **✓** Es leitet aus Mustern in seinen Beispielen ein Label ab.
- Es sucht im Web nach demselben Foto.
- Es denkt über das Foto nach, wie ein Mensch es tun würde.

**Warum:** Es wendet Muster aus seinen beschrifteten Beispielen auf eine neue Eingabe an: Das ist Inferenz. Ein Speicher voller Antworten könnte nur Fotos zurückgeben, die er schon hatte, eine Websuche findet Seiten, die es gibt, und das System hat Muster, nicht die Gedanken eines Menschen.

### Frage 2 von 3

Ein Tool, das ein Support-Postfach sortiert, hat aus vielen Nachrichten zu Rechnungen und nur wenigen zum Versand gelernt. Eine neue Nachricht ist unklar: Sie könnte beides sein. Was ist das wahrscheinliche Ergebnis?

- **✓** Es ordnet sie eher als Nachricht zu Rechnungen ein.
- Es ordnet sie mit gleicher Wahrscheinlichkeit der einen oder der anderen Art zu, weil es beide gesehen hat.
- Es weigert sich, die Nachricht zu sortieren.

**Warum:** Seine Beispiele neigen zu Rechnungen, also neigen seine Antworten nahe der Grenze ebenfalls dorthin, so wie die Lieferung mit Orangen die Grenze in der Visualisierung verschoben hat. Es hat Nachrichten zum Versand gesehen, aber zu wenige, um sich zu behaupten. Und ein Klassifikator wie der in der Visualisierung wählt immer eine seiner Arten: Er kann sich nicht weigern.

### Frage 3 von 3

Warum hat der Klassifikator in der Visualisierung angefangen, eine typische Zitrone Orange zu nennen?

- Die Zitrone hat Größe und Farbe geändert.
- **✓** Die Lieferung hat Orangen ganz nah an sie gelegt, sodass die meisten ihrer nächsten Nachbarn Orangen wurden.
- Jemand hat dem Klassifikator gesagt, dass die richtige Antwort Orange ist.
- Mehr Beispiele machen einen Klassifikator immer ungenauer.

**Warum:** Die Zitrone und die Regel (die 3 nächsten entscheiden) blieben gleich; nur die Beispiele änderten sich. Niemand hat ihm die Antwort gesagt. Mehr Beispiele sind an sich nicht das Problem: unausgewogene schon.

## Was du jetzt kannst

- Mit einfachen Worten erklären, dass ein KI-System Ergebnisse aus Mustern in seinen Beispielen ableitet.
- Training (wenn ein System aus seinen Beispielen lernt) und Nutzung (wenn es das Gelernte anwendet) auseinanderhalten.
- Sagen, warum ein KI-System kein Verstand, keine Datenbank und keine Suchmaschine ist.
- Erklären, warum einseitige Beispiele zu einseitigen Antworten führen.

---

[Zurück zu KI-Grundlagen](README.md) · [Weiter: Wie ein Modell schreibt](02-how-a-model-writes.md)
