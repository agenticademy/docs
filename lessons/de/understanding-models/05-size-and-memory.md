# Größe und Speicher

Modelle verstehen · Kapitel 5 von 8 · 15 Minuten

Den Speicher, den ein Modell braucht, bestimmen seine Parameter und die Bits, mit denen jeder gespeichert wird, plus Platz für seinen Kontext; ein Mixture-of-Experts-Modell nutzt für jedes Token nur wenige seiner Parameter, hält aber alle im Speicher.

## Ziele

- Schiebe Llama-3.1-8B auf einen 4-Bit-Typ und sieh, wie seine Datei schrumpft.
- Prüfe, welche Versionen in 8, 16, 24 und 80 GB passen.
- Vergleiche die Gesamt- und aktiven Parameter von drei Mixture-of-Experts-Modellen.
- Bestehe den Test.

## Der Text

### Parameter brauchen Speicher

In Kapitel 1 hast du Parameter kennengelernt, die Zahlen, die ein Modell gelernt hat, auch Gewichte genannt. Oft nennt der Name ihre Zahl: Llama-3.1-8B hat etwa 8 Milliarden. Damit ein Modell läuft, muss es alle Gewichte im Speicher halten: dem schnellen Speicher, mit dem ein Computer arbeitet, oder dem Speicher einer Grafikkarte (einer GPU, eines Chips, der viele Rechnungen auf einmal macht). Speicher wird in Bytes gezählt, und ein Byte hat 8 Bit. Die Gewichte brauchen also Parameter × Bits pro Gewicht ÷ 8 Bytes. Mit 16 Bit pro Gewicht belegen 8 Milliarden Gewichte 16 GB, also 16 Milliarden Bytes.

### Weniger Bits, etwas schlechter

Quantisierung, aus Kapitel 4, speichert jedes Gewicht mit weniger Bits. Weniger Bits können weniger Werte fassen, also wird jedes Gewicht gröber gerundet: Die Datei schrumpft, und die Antworten werden etwas schlechter, bei den wenigsten Bits stärker. Die Grafik nutzt Messungen von llama.cpp, einem kostenlosen Programm, das Modelle auf deinem eigenen Computer ausführt. Die Namen seiner Typen nennen die Bits: Q4\_K\_M hält die meisten Gewichte in 4 Bit und manche in mehr, plus ein wenig zusätzlich für jeden Block von Gewichten: im Schnitt 4,89 Bit. Es gibt Größen in GiB an: 1 GiB sind 1.073.741.824 Bytes, etwa 7 % mehr als 1 GB.

### Platz zum Arbeiten

Ein Modell passt, wenn seine Gewichte plus etwas Platz in den Speicher passen, den du hast. Auf einem Laptop teilt sich dieser Speicher alles andere, was gerade läuft. Der Platz ist für den Kontext: Während ein Modell arbeitet, behält es, was es für jedes Token berechnet hat, und dieser Vorrat wächst mit jedem Token, bis zum Kontextfenster aus Kapitel 2.

### Viele Experten, wenige auf einmal

Ein Mixture-of-Experts-Modell teilt die meisten seiner Schichten in Experten auf, kleinere Gruppen von Gewichten. Für jedes Token wählt ein kleiner Teil, der Router, ein paar Experten aus, und nur sie arbeiten. Ihre Parameter plus die Teile, die jedes Token nutzt, sind die aktiven Parameter; alle zusammen sind die Gesamtzahl. Ein Token kostet etwa so viel Arbeit wie in einem Modell von der aktiven Größe, aber der Router kann für das nächste Token jeden Experten wählen, also muss die Gesamtzahl im Speicher sein.

### Geschlossen und offen

Geschlossene Modelle wie Claude, GPT, Gemini und Grok veröffentlichen keine Gewichte, und ihre Modellseiten nennen keine Parameterzahlen, also stammt jede Parameterzahl in der Grafik von der eigenen Seite eines offenen Modells, Stand 2026-11-03.

## Probier es aus: Passt es?

Schiebe ein Modell auf weniger Bits, probiere vier Speichergrößen aus und sieh dann, wie viel eines großen Modells für jedes Token arbeitet.

https://agenticademy.org/de/paths/understanding-models/05-size-and-memory

## Check

Dieses Mixture-of-Experts-Modell ist auf etwa 4,9 Bit pro Gewicht quantisiert. Welcher dieser Speicher ist der kleinste, der seine Gewichte hält?

Das Modell:

```
Modell: gpt-oss-120b, offene Gewichte
Gesamt: 117 Milliarden Parameter
Aktiv: 5,1 Milliarden pro Token
Zahlen: Model Card, 2026-11-03
Quantisiert: 4,9 Bit pro Gewicht

Speicher: 8 GB · 16 GB · 24 GB · 80 GB
```

- 8 GB: Nur die 5,1 Milliarden aktiven Parameter müssen im Speicher sein, etwa 3,1 GB.
- 16 GB: die 5,1 Milliarden aktiven Parameter mit je 16 Bit, etwa 10,2 GB.
- **✓** 80 GB: Alle 117 Milliarden Parameter müssen im Speicher sein, etwa 72 GB.
- Keiner davon: 117 Milliarden × 4,9 Bit sind etwa 573 GB.

**Warum:** Der Router kann für das nächste Token jeden Experten wählen, also müssen alle 117 Milliarden Parameter im Speicher sein, nicht nur die 5,1 Milliarden, die ein Token nutzt: 117 Milliarden × 4,9 Bit ÷ 8 Bit pro Byte ≈ 72 GB, sodass von den 80 etwa 8 GB für den Kontext bleiben. 8 GB und 16 GB würden nur den aktiven Teil halten, mit 4,9 oder mit 16 Bit. 573 GB vergisst, durch 8 zu teilen, die Bits in einem Byte.

## Was du jetzt kannst

- Den Speicher, den die Gewichte eines Modells brauchen, aus seinen Parametern und Bits pro Gewicht schätzen.
- Sagen, was Quantisierung spart und was sie kostet.
- Erkennen, ob ein Modell in einen gegebenen Speicher passt, mit Platz für seinen Kontext.
- Gesamt- und aktive Parameter in einem Mixture-of-Experts-Modell erklären und warum die Gesamtzahl den Speicher bestimmt.

---

[Zurück zu Modelle verstehen](README.md) · [Weiter: Offene und geschlossene Modelle](06-open-and-closed.md)
