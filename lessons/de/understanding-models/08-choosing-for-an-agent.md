# Ein Modell für einen Agenten wählen

Modelle verstehen · Kapitel 8 von 8 · 18 Minuten

Ein Modell wählen heißt: die streichen, die an einer harten Grenze scheitern, und dann Fähigkeit, Tempo, Preis pro Aufgabe, Kontext und Zuverlässigkeit bei Tools für die Arbeit eines Agenten abwägen.

## Ziele

- Wähle einen Agenten, für den du ein Modell aussuchst.
- Wende seine harten Grenzen an und sieh, welche Modelle wegfallen.
- Wäge ab, was übrig ist, ordne es und berechne den Preis eines Arbeitsmonats.
- Bestehe den Test.

## Der Text

### Das passende, nicht das beste

Es gibt kein bestes Modell, nur das, das am besten zur Arbeit eines Agenten passt. Die Wahl braucht zwei Durchgänge. Zuerst kommen die harten Grenzen: Anforderungen, ohne die der Agent nicht auskommt. Ein Modell, das an einer scheitert, fällt weg, egal wie stark oder günstig es ist. Dann wägst du ab, was übrig ist: Du entscheidest, wie viel jede Eigenschaft für diese Arbeit zählt, und ordnest die Modelle danach.

### Harte Grenzen

Modalität: die Arten von Ein- und Ausgabe, die ein Modell verarbeitet, etwa Text, Bilder oder Ton. Kontextfenster: wie viele Tokens es auf einmal liest, wie in Kapitel 2. Datenrichtlinie: ob der Anbieter seine Modelle mit dem trainieren darf, was du sendest, und die Aufbewahrung, wie lange er es behält. Region: wo deine Daten verarbeitet werden; manche Regeln, etwa die der EU, legen Wert darauf, wo. Lizenz und Zugang: was du mit einem Modell tun darfst und wer es überhaupt nutzen darf. Offene Gewichte können auch auf deinem eigenen Rechner laufen, sogar ohne Internet.

### Was du abwägst

Fähigkeit: wie gut es die Arbeit erledigt. Benchmarks wie in Kapitel 7 helfen, wenn ihre Aufgaben deinen ähneln, aber deine eigene Evaluation sagt mehr: ein paar Dutzend echte Aufgaben aus der Arbeit, von dir bewertet. Latenz: wie lange du auf eine Antwort wartest; mehr Nachdenken bringt meist bessere Antworten und längere Wartezeiten.

Preis pro Aufgabe: was eine erledigte Aufgabe kostet. Ein Agent ruft sein Modell pro Aufgabe mehrmals auf, und jeder Aufruf ist eine Runde, abgerechnet nach ihren Tokens rein und raus. Ein günstiger Preis pro Token kann trotzdem eine teure Aufgabe ergeben. Zuverlässigkeit bei Tool-Aufrufen: wie oft das Modell Tools richtig aufruft, mit den richtigen Eingaben. Rate Limits: wie viele Anfragen und Tokens ein Anbieter pro Minute annimmt.

### In der Grafik

Du wählst einen von vier Agenten. Seine harten Grenzen streichen Modelle, deine Gewichtung ordnet den Rest, und ein Rechner macht aus echten Listenpreisen eine Monatsrechnung. Eine Musterantwort zeigt dann, worauf ihre Wahl verzichtet. Die Agenten und die Größe ihrer Aufgaben sind erfunden. Die Modelle, Preise, Kontextfenster, Eingaben, Lizenzen und Scores sind echt, aus einer Momentaufnahme vom 2026-11-03. Die Momentaufnahme enthält keine Regionen und nur einige Datenbedingungen, deshalb steht bei manchen Grenzen „Prüfen“; sie enthält weder Geschwindigkeiten noch Rate Limits, die testest du also selbst.

## Probier es aus: Wähle ein Modell für diesen Agenten

Wähle einen Agenten, wende seine harten Grenzen nacheinander an, wäge ab, was übrig ist, berechne einen Arbeitsmonat und vergleiche dann mit einer Musterantwort.

https://agenticademy.org/de/paths/understanding-models/08-choosing-for-an-agent

## Check

### Frage 1 von 3

Ein Support-Bot für eine europäische Bank muss Kundendaten in der EU halten. Zwei Modelle können die Arbeit; das günstigere läuft nur auf Servern außerhalb der EU. Was kommt zuerst?

- Das günstigere nehmen: Der Preis zählt am meisten.
- **✓** Das streichen, das die Daten nicht in der EU halten kann, und dann den Rest vergleichen.
- Das mit dem höheren Score in der Rangliste nehmen.
- Beide nutzen und abstimmen lassen.

**Warum:** Daten in der EU zu halten ist eine harte Grenze: Ein Modell, das daran scheitert, fällt weg, egal wie günstig oder stark es ist. Preis und Scores ordnen nur die zugelassenen Modelle. Beide abstimmen zu lassen, würde die Daten trotzdem aus der EU schicken.

### Frage 2 von 3

Modell B kostet pro Token doppelt so viel wie Modell A. In deiner Evaluation erledigt B eine Aufgabe in 2 Runden, und A braucht 5 Runden gleicher Größe. Welches kostet pro Aufgabe weniger?

- A, weil sein Preis pro Token halb so hoch ist wie der von B.
- **✓** B, weil 2 Runden zum doppelten Preis weniger kosten als 5 Runden.
- Keins: Bei gleich vielen Tokens pro Runde entscheidet der Preis pro Token.
- Ohne Score aus einer Rangliste lässt sich das nicht sagen.

**Warum:** Der Preis pro Aufgabe ist der Preis pro Token mal die Tokens, die eine Aufgabe verbraucht. Kostet eine Runde bei A 1, kostet sie bei B 2, also kostet die Aufgabe bei A 5 und bei B 4. Der Preis pro Token allein übersieht die Zahl der Runden, und ein Score aus einer Rangliste zählt deine Runden überhaupt nicht; deine eigene Evaluation schon.

### Frage 3 von 3

Das oberste Modell deiner Auswahlliste hat den besten unabhängigen Score, aber die Momentaufnahme sagt nichts über sein Tempo oder darüber, wie zuverlässig es Tools aufruft. Was tust du, bevor du dich festlegst?

- Nichts: Der beste Score heißt, dass es auch das schnellste und zuverlässigste ist.
- **✓** Eine eigene Evaluation machen: echte Aufgaben aus der Arbeit, die Antworten stoppen und jeden Tool-Aufruf prüfen.
- Stattdessen das günstigste Modell nehmen.
- Auf die Rangliste des nächsten Monats warten.

**Warum:** Ein Score misst die Aufgaben seines Benchmarks bei einer Einstellung; er stoppt keine Antworten bei deiner Arbeit und prüft deine Tools nicht. Deine eigene Evaluation, ein paar Dutzend echte Aufgaben, die du selbst bewertest, misst genau das. Nur nach Preis zu wählen oder auf eine neuere Rangliste zu warten, lässt dieselbe Lücke.

## Was du jetzt kannst

- Harte Grenzen, die ein Modell streichen, von den Eigenschaften trennen, die du abwägst.
- Aus Tokens, Runden und Listenpreisen einen Preis pro Aufgabe und pro Monat berechnen.
- Eine Auswahlliste nach dem ordnen, was für einen Agenten zählt, und sagen, worauf die Wahl verzichtet.
- Benennen, was die Seite eines Modells dir nicht sagen kann, und es in deiner eigenen Evaluation testen.

---

[Zurück zu Modelle verstehen](README.md)
