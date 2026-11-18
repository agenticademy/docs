# Das nächste Token wählen

Modelle verstehen · Kapitel 3 von 8 · 14 Minuten

Ein Modell gibt jedem möglichen nächsten Token einen Score, macht aus den Scores Wahrscheinlichkeiten und zieht eins zufällig; Temperatur, Top-k und Top-p verändern die Ziehung, und Basis-, Instruct- und Reasoning-Modelle sind darauf trainiert, unterschiedlich zu schreiben.

## Ziele

- Sieh dir die 20 wahrscheinlichsten nächsten Tokens für einen Prompt an.
- Verschieb Temperatur und Top-p und zieh noch einmal.
- Stell einem Basismodell und einem Instruct-Modell dieselbe Frage.
- Besteh den Check.

## Der Text

### Ein Token nach dem anderen

Im letzten Kapitel wurde Text zu Tokens. Ein Modell schreibt auf dieselbe Weise: Es liest den ganzen bisherigen Text, wählt ein nächstes Token, hängt es an den Text an und beginnt von vorn. Eine Antwort aus 100 Tokens braucht 100 solcher Schritte.

### Aus Scores werden Wahrscheinlichkeiten

Bei jedem Schritt gibt das Modell jedem Token, das es kennt, einen Score, Logit genannt. SmolLM2-360M, das kleine offene Modell in diesem Kapitel, kennt 49.152 Tokens, also gibt es 49.152 Scores. Dann werden die Scores in Chancen umgerechnet, auch Wahrscheinlichkeiten genannt, die zusammen 100 % ergeben. Ein höherer Score bedeutet eine größere Wahrscheinlichkeit: Ein Punkt mehr bedeutet etwa die 2,7-fache Wahrscheinlichkeit.

### Ein Zufallszug, mit Einstellungen

Dann wird ein Token zufällig gezogen, nach diesen Wahrscheinlichkeiten, wie ein Los aus einem Beutel, in dem wahrscheinliche Tokens mehr Lose haben. Deshalb kann derselbe Prompt verschiedene Antworten bekommen. Drei Einstellungen verändern die Ziehung. Die Temperatur teilt jeden Score, bevor er zur Wahrscheinlichkeit wird: Unter 1 werden wahrscheinliche Tokens noch wahrscheinlicher, über 1 gleichen sich die Wahrscheinlichkeiten an. Top-k behält nur die k wahrscheinlichsten Tokens. Top-p behält die wahrscheinlichsten Tokens, bis ihre Wahrscheinlichkeiten zusammen p ergeben, also kappt ein Top-p von 0,9 die lange Reihe der unwahrscheinlichen. Was übrig bleibt, wird wieder auf 100 % hochskaliert.

### Basis- und Instruct-Modelle

Ein Basismodell hat nur gelernt, Text fortzusetzen, also bekommt eine Frage vielleicht weitere Fragen. Ein Instruct-Modell wurde zusätzlich mit Gesprächen trainiert, geschrieben in einem Chat-Format, das markiert, wer spricht. Es antwortet und schreibt dann ein End-Token, ein besonderes Token, das bedeutet, dass es fertig ist.

### Reasoning-Modelle

Ein Reasoning-Modell schreibt zuerst seinen Lösungsweg, auf dieselbe Weise Token für Token, und dann die Antwort. Stand 2026-11-03 machen geschlossene Modelle wie Claude Opus 5.5 und GPT-6.1 Sol das Reasoning zu einer Einstellung, und offene wie DeepSeek-R1 und Olmo 3 7B Think erscheinen als eigene Reasoning-Modelle.

### In der Visualisierung

Die Zahlen in der Visualisierung wurden mit SmolLM2-360M und seiner Instruct-Version aufgezeichnet, beide von Hugging Face. Wenn dein Browser WebGPU hat, eine Möglichkeit für Webseiten, die Grafikkarte zu nutzen, kannst du das Instruct-Modell auch herunterladen und live laufen lassen.

## Probier es aus: Ziehung des nächsten Tokens

Wähl einen Prompt und lies die Wahrscheinlichkeiten, dann form sie und zieh, ein Token nach dem anderen. Zum Schluss stell dem Basis- und dem Instruct-Modell dieselbe Frage.

https://agenticademy.org/de/paths/understanding-models/03-next-token

## Check

Du ziehst das nächste Token 100 Mal mit diesen Einstellungen. Was bekommst du?

Die Einstellungen:

```
Prompt: The capital of France is
Modell: SmolLM2-360M (Basis)

Nächstes Token   Wahrscheinlichkeit
Paris            20,3 %
the              19,6 %
located          10,5 %
situated          4,7 %
called            3,7 %
… 15 weitere, dann der ganze Rest

Temperatur: 1
Top-k:      aus
Top-p:      0,5
```

- „Paris“ alle 100 Mal: Es ist das wahrscheinlichste Token.
- „Paris“ und „the“, je etwa 50 Mal.
- **✓** „Paris“, „the“ und „located“, etwa 40, 39 und 21 Mal.
- Jedes der 49.152 Tokens, jedes so oft, wie seine Wahrscheinlichkeit sagt.

**Warum:** Top-p behält die wahrscheinlichsten Tokens, bis ihre Wahrscheinlichkeiten p erreichen. „Paris“ und „the“ ergeben zusammen 39,9 %, weniger als 50 %; mit „located“ sind es 50,4 %, also bleiben diese drei, und der Rest wird gekappt. Die drei werden wieder auf 100 % hochskaliert: etwa 40 %, 39 % und 21 %. Jedes Mal „Paris“ bräuchte eine Einstellung, die ein einziges Token behält, etwa Top-k auf 1 oder Top-p auf 0,2, und jedes Token könnte nur mit Top-p auf 1 vorkommen.

## Was du jetzt kannst

- Erklären, wie ein Modell Scores in Wahrscheinlichkeiten umrechnet und das nächste Token zieht.
- Sagen, was Temperatur, Top-k und Top-p mit den Wahrscheinlichkeiten machen.
- Die Ausgabe eines Basismodells von der eines Instruct-Modells unterscheiden und sagen, was ein Reasoning-Modell hinzufügt.

---

[Zurück zu Modelle verstehen](README.md) · [Weiter: Woher Modelle kommen](04-where-models-come-from.md)
