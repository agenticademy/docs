# Woher Modelle kommen

Modelle verstehen · Kapitel 4 von 8 · 15 Minuten

Ein Modell entsteht in Stufen, von den Daten und dem Pre-Training über Fine-Tuning, Feedback und Destillation bis zur Quantisierung, und bei offenen Modellen hinterlassen viele Stufen eine Datei, die du dir ansehen kannst.

## Ziele

- Verfolge Olmo 3, ein vollständig offenes Modell, von seinen Daten bis zur Veröffentlichung.
- Verfolge DeepSeek-R1 bis zu einem kleinen Modell, das daraus destilliert wurde.
- Verfolge Gemma 4 von seinem Basismodell bis zu einer quantisierten Datei.
- Besteh den Check.

## Der Text

### Daten, Tokens und Pre-Training

Jedes Modell beginnt mit Daten: großen Sammlungen von Text, Code und oft Bildern, gefiltert und ohne doppelte Kopien. Ein Tokenizer zerlegt sie, wie in Kapitel 2, in Tokens.

Das Pre-Training zeigt dem Modell Billionen von Tokens und passt seine Gewichte an, auch Parameter genannt: die Zahlen, die es lernt. Es wird besser darin, das nächste Token vorherzusagen, die Aufgabe aus Kapitel 3. Labore speichern unterwegs Checkpoints, Kopien der Gewichte. Manche, wie Olmo 3, enden mit Mid-Training, einer kürzeren Phase mit einer kleineren, ausgewählten Mischung, und einer Phase mit langen Texten. Das Ergebnis ist ein Basismodell, das Text fortsetzt, aber keinen Anfragen folgt.

### Post-Training

Das Post-Training bringt ein Basismodell dazu, Anfragen zu folgen. Überwachtes Fine-Tuning (SFT) trainiert es mit Beispielen für Anfragen mit guten Antworten.

Reinforcement Learning (RL) trainiert es dann mit Belohnungen: Antworten mit gutem Score werden wahrscheinlicher. Bei RLHF, RL mit menschlichem Feedback, vergleichen Menschen Antworten, und die Scores ergeben sich aus ihrer Wahl. Bei RLAIF gibt ein anderes Modell das Feedback, manchmal geleitet von einer schriftlichen Liste von Prinzipien, die Verfassung heißt. DPO, direkte Präferenzoptimierung, lernt in einem einfacheren Schritt aus Paaren einer besseren und einer schlechteren Antwort.

Für Reasoning-Modelle kommt RL mit überprüfbaren Belohnungen (RLVR) hinzu: Eine Antwort wird nur belohnt, wenn ein Programm sie bestätigt, etwa ein Rechenergebnis oder Code, der seine Tests besteht.

### Kleiner und leichter

Destillation trainiert ein kleines Modell, den Schüler, mit Antworten eines großen Modells, des Lehrers. Quantisierung speichert jedes Gewicht mit weniger Bits, den Nullen und Einsen im Computerspeicher, zum Beispiel 4 statt 16, sodass die Datei schrumpft und auf kleinere Rechner passt, mit einem kleinen Qualitätsverlust. Quantisierungsbewusstes Training (QAT) trainiert ein Modell so, dass es beim Quantisieren weniger verliert. GGUF ist ein Dateiformat, das Apps lesen, die Modelle auf deinem eigenen Computer ausführen.

### Getestet und beschrieben

Vor der Veröffentlichung bewerten Labore ein Modell mit Benchmarks, festen Sammlungen von Testfragen, und machen Red-Teaming: Tester versuchen, es dazu zu bringen, Schaden anzurichten. Eine Model Card beschreibt das Modell, sein Training und seine Ergebnisse; eine System Card behandelt auch die Schutzmaßnahmen darum herum.

Offene Modelle veröffentlichen ihre Gewichte; vollständig offene veröffentlichen auch ihre Daten und ihren Code. Geschlossene Modelle wie Claude, GPT, Gemini und Grok werden ebenfalls trainiert, aber ihre Hersteller veröffentlichen Berichte, keine Gewichte, deshalb lassen sich ihre Stammbäume nicht Datei für Datei verfolgen. Die Visualisierung verfolgt drei offene.

## Probier es aus: Der Stammbaum eines Modells

Verfolge drei echte Stammbäume von oben, eine Stufe nach der anderen. Jede Stufe zeigt, was sie genutzt und erzeugt hat und wo das veröffentlicht ist.

https://agenticademy.org/de/paths/understanding-models/04-where-models-come-from

## Check

Bring die Stufen, in denen ein Modell entsteht, in die richtige Reihenfolge, von der ersten bis zur letzten.

1. Die Daten sammeln und bereinigen und in Tokens zerlegen.
2. Mit der Vorhersage des nächsten Tokens vortrainieren: Das Ergebnis ist ein Basismodell.
3. Es per Fine-Tuning mit Beispielen für Anfragen mit guten Antworten trainieren.
4. Es mit Belohnungen weitertrainieren: aus Feedback und aus Antworten, die ein Programm prüfen kann.
5. Aus seinen Antworten ein kleineres Schülermodell destillieren.
6. Die Gewichte des Schülers auf weniger Bits quantisieren.
7. Das Ergebnis evaluieren und Red-Teaming machen, dann seine Model Card veröffentlichen.

**Warum:** Das Pre-Training liest Tokens, also kommen die Daten und die Tokens zuerst, und es ergibt ein Basismodell. Fine-Tuning mit Beispielen bringt diesem Modell bei, Anfragen zu folgen, und Belohnungen formen es weiter, wie bei SFT, DPO und RLVR von Olmo 3. Nur ein fertiges Modell kann einen Schüler unterrichten, und das Quantisieren verkleinert danach die Datei des Schülers. Die letzten Tests und das Red-Teaming kommen vor der Veröffentlichung, und die Model Card berichtet darüber.

## Was du jetzt kannst

- Die Stufen, in denen ein Modell entsteht, der Reihe nach nennen, von den Daten bis zur Model Card.
- Erklären, was Fine-Tuning, Training mit Feedback, Destillation und Quantisierung jeweils verändern.
- Den Stammbaum eines offenen Modells anhand seiner Model Card Stufe für Stufe verfolgen.
- Sagen, warum sich ein vollständig offenes Modell auf jeder Stufe prüfen lässt und ein geschlossenes nicht.

---

[Zurück zu Modelle verstehen](README.md) · [Weiter: Größe und Speicher](05-size-and-memory.md)
