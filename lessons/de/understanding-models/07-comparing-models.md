# Modelle vergleichen

Modelle verstehen · Kapitel 7 von 8 · 15 Minuten

Benchmarks und Ranglisten helfen dir, Modelle zu vergleichen, aber Fehlermargen, Aufwandsstufen, Kosten, durchgesickerte Tests, ausgereizte Tests, das Testprogramm und wer den Test durchgeführt hat, ändern alle, was ein Score bedeutet.

## Ziele

- Lies eine Rangliste mit ihren Intervallen und finde die Gleichstände.
- Vergleiche ein Modell auf fünf Aufwandsstufen mit einem Wechsel zu einem anderen Modell.
- Ordne vier Benchmark-Fallen Beispielen zu.
- Besteh den Check.

## Der Text

### Benchmarks und Ranglisten

Ein Benchmark ist eine feste Menge von Testaufgaben mit einer Methode, die Antworten zu bewerten. Eine Rangliste ordnet Modelle nach solchen Scores. Die Visualisierung nutzt zwei echte. Auf Arena schicken Menschen einen Prompt, lesen zwei Antworten von Modellen, die sie nicht sehen, und stimmen für die, die ihnen lieber ist; Millionen Stimmen werden zu einem Score für jedes Modell. Artificial Analysis, ein unabhängiges Unternehmen, führt selbst neun Benchmarks durch und fasst sie zu einer Zahl zusammen, seinem Intelligence Index.

### Jeder Score hat eine Marge

Ein Score aus Stimmen oder aus einer Stichprobe von Aufgaben ist eine Schätzung. Arena schreibt ihn als Score ± Marge: Der wahre Score liegt sehr wahrscheinlich in diesem Bereich, dem 95-%-Intervall. Weniger Stimmen ergeben einen breiteren Bereich. Wenn sich die Bereiche zweier Modelle überlappen, kann die Rangliste sie nicht unterscheiden; behandle sie also als gleichauf, egal auf welchem Platz sie stehen.

### Einstellungen und Kosten

Viele Modelle haben eine Aufwandsstufe, etwa niedrig, mittel, hoch oder max: wie viel das Modell nachdenken darf, bevor es antwortet, wie in Kapitel 3. Mehr Aufwand bringt oft einen höheren Score, aber das Modell schreibt mehr Tokens, also kostet jede Aufgabe mehr und dauert länger. Artificial Analysis zeigt auch Kosten pro Aufgabe: was es gekostet hat, das Modell auf einer seiner Aufgaben laufen zu lassen, in US-Dollar. Ein Score sagt wenig ohne seine Einstellung und seine Kosten.

### Fallen

Kontamination: Die Fragen und Antworten des Tests sind in die Trainingsdaten gelangt, also erinnert sich ein Modell vielleicht an sie, statt sie zu lösen. Sättigung: Die besten Modelle erreichen alle fast das Maximum, also trennt der Test sie nicht mehr. Der Harness ist das Programm, das den Test ausführt, mit seinen Anweisungen, Tools und Wiederholungen; ein besserer Harness kann den Score desselben Modells erhöhen. Der eigene Score eines Herstellers wird auf seine eigene Art gemessen und kann von einem unabhängigen abweichen. Versionsdrift: Tests ändern sich zwischen Versionen, also braucht ein Score seine Version und sein Datum.

### In der Visualisierung

Die Zahlen von Arena und Artificial Analysis sind echt, so wie veröffentlicht, in einer Momentaufnahme vom 2026-11-03. Die Kostenachse des Diagramms ist logarithmisch: 0,10 $ bis 1 $ nimmt dieselbe Breite ein wie 1 $ bis 10 $, sodass günstige und teure Modelle beide Platz haben. Die Beispiele im letzten Schritt sind erfunden.

## Probier es aus: Benchmark-Scores lesen

Finde die Gleichstände in einer echten Rangliste, steig die Aufwandsleiter eines Modells hinauf und ordne dann vier Fallen erfundenen Beispielen zu.

https://agenticademy.org/de/paths/understanding-models/07-comparing-models

## Check

### Frage 1 von 3

In einem Beispiel erreicht Modell A in einer Rangliste 1.310 ± 8 und Modell B 1.302 ± 9. Was kannst du sagen?

- A ist klar besser als B.
- **✓** Ihre Intervalle überlappen sich, also kann die Rangliste sie nicht unterscheiden.
- B ist besser, weil sein Intervall breiter ist.
- Die Scores müssen falsch sein.

**Warum:** Das Intervall von A reicht von 1.302 bis 1.318, das von B von 1.293 bis 1.311. Die Bereiche überlappen sich, also kann der Abstand Rauschen sein: Behandle sie als gleichauf, nicht A als klar besser. Ein breiteres Intervall bedeutet weniger Sicherheit, etwa weniger Stimmen, kein besseres Modell. Und nichts spricht dafür, dass die Scores falsch sind: Es sind Schätzungen mit ihren Margen.

### Frage 2 von 3

Ein Bericht sagt, Modell X erreicht in einem Index 5 Punkte mehr als Modell Y. Was solltest du fragen, bevor du zu X wechselst?

- Nichts: 5 Punkte sind ein klarer Sieg.
- **✓** Auf welcher Aufwandsstufe jedes lief und was jedes pro Aufgabe gekostet hat.
- Welcher der beiden Hersteller größer ist.
- Welches Modell zuerst erschienen ist.

**Warum:** In der Visualisierung hat niedrig bis max jedes Modell mit fünf Stufen um 20 bis 29 Punkte bewegt, bei etwa 8- bis 20-fachen Kosten pro Aufgabe. Ein Abstand von 5 Punkten kann also allein von den Stufen kommen, und der höhere Score kann pro Aufgabe weit mehr kosten. Die Größe des Herstellers und das Erscheinungsdatum sagen über beides nichts.

### Frage 3 von 3

Ein Blogbeitrag stellt den Score von Modell A in der aktuellen Version eines Tests neben den Score von Modell B in der Version von vor einem Jahr. Die Zahl von A ist höher. Was kannst du daraus schließen?

- A ist besser, weil seine Zahl höher ist.
- B ist besser, weil es zuerst getestet wurde.
- **✓** Noch nicht viel: Verschiedene Versionen nutzen verschiedene Aufgaben, also stehen die beiden Zahlen nicht auf derselben Skala.
- Die neuere Version muss leichter sein.

**Warum:** Das ist Versionsdrift. Eine neue Version ändert die Aufgaben, wie viele es sind oder wie sie gewichtet werden, also stehen ihre Scores nicht auf der alten Skala. Am 2026-11-03 zum Beispiel war der Index von Artificial Analysis bei Version 5.0.1, mit neun Evaluationen. Vergleiche Modelle in derselben Version, gelesen am selben Datum. Zuerst getestet zu werden macht ein Modell nicht besser, und nichts spricht dafür, dass die neuere Version leichter ist.

## Was du jetzt kannst

- Die Intervalle einer Rangliste lesen und sagen, welche Modelle sie nicht unterscheiden kann.
- Modelle auf benannten Aufwandsstufen vergleichen, zusammen mit ihren Kosten pro Aufgabe.
- Kontamination, Sättigung, Harness-Effekte und eigene Scores eines Herstellers in einer Behauptung erkennen.
- Nach Version, Datum und Durchführendem fragen, bevor man einem Score traut.

---

[Zurück zu Modelle verstehen](README.md) · [Weiter: Ein Modell für einen Agenten wählen](08-choosing-for-an-agent.md)
