# Tokens

Modelle verstehen · Kapitel 2 von 8 · 14 Minuten

Modelle lesen und schreiben Text als Tokens, jede Modellfamilie zerlegt Text auf ihre eigene Weise, und sowohl Preise als auch Kontextgrenzen werden in Tokens gezählt.

## Ziele

- Zerleg einen Satz mit den Tokenizern von sechs Modellfamilien.
- Wechsle zu Spanisch, Japanisch und Code und vergleiche die Zahlen.
- Besteh den Check.

## Der Text

### Aus Text werden Tokens

Im letzten Kapitel hast du viele Arten von Modellen kennengelernt. Die, die mit Text arbeiten, fangen alle gleich an. Ein Tokenizer, ein kleines Programm, das zu jedem Modell gehört, zerlegt den Text in Tokens: Stücke, die oft ein ganzes Wort mit dem Leerzeichen davor sind und manchmal ein Teil eines Worts, ein Symbol oder eine Ziffer. Jedes Token wird dann gegen eine Zahl getauscht, seine Token-ID. Das Modell liest nur diese Zahlen. Wenn es schreibt, gibt es Token-IDs zurück, und der Tokenizer macht daraus Text.

### Jede Familie hat ihr eigenes Vokabular

Die Liste der Stücke, die ein Tokenizer kennt, ist sein Vokabular. Es entsteht aus einer großen Textprobe, bevor das Modell trainiert wird: Stücke, die dort oft vorkommen, werden jeweils zu einem Token. Jede Modellfamilie, etwa GPT, Llama oder Qwen, hat ihren eigenen Tokenizer, und eine neue Generation bekommt vielleicht einen neuen. Die sechs im Lab kennen zwischen 32.768 und 262.144 Stücke.

Ein Wort, das nicht im Vokabular steht, wird aus kleineren Stücken gebaut. Ganz am Ende stehen Bytes, die kleinen Codes, mit denen ein Computer jedes Zeichen speichert. Ein japanisches Schriftzeichen oder ein Emoji braucht drei oder vier Bytes, also braucht ein Tokenizer, dem es fehlt, mehrere Tokens für ein einziges Zeichen.

### Warum die Anzahl wichtig ist

Anbieter berechnen ihre Modelle pro Million Tokens: die Tokens, die du sendest, genannt Eingabe, und die Tokens, die das Modell schreibt, genannt Ausgabe. Auch das Kontextfenster eines Modells, die größte Textmenge, die es auf einmal aufnehmen kann, wird in Tokens gezählt. Text, der mehr Tokens braucht, kostet mehr und füllt das Fenster früher. Im Lab braucht dieselbe Anfrage in jeder Familie auf Spanisch und Japanisch mehr Tokens als auf Englisch, und bei Code und Emoji passen weniger Zeichen in jedes Token als bei Englisch.

### Was im Lab steckt

Sechs Tokenizer: der von OpenAI für GPT-4o, ein geschlossenes Modell, und die von fünf Familien mit offenen Gewichten, deren Modelldateien veröffentlicht sind: Gemma 4 von Google, Llama 3 von Meta, Qwen3, DeepSeek-V3 und Mistral v3. Ihre Zerlegungen wurden mit ihren echten Tokenizer-Dateien aufgezeichnet. Der letzte Schritt ist optional: Er lädt die Dateien herunter, die du auswählst, zeigt vorher ihre Größe und zerlegt deinen eigenen Text auf dieser Seite.

## Probier es aus: Tokenizer-Lab

Zerleg einen Satz mit sechs Tokenizern, dann wechsle den Text und sieh zu, wie sich die Zahlen ändern.

https://agenticademy.org/de/paths/understanding-models/02-tokens

## Check

Dieses deutsche Wort bedeutet „Nachricht, die eine Tischreservierung bestätigt“. GPT-4o macht aus dem englischen Satz des Labs 10 Tokens, etwa eines pro Wort. Wie viele macht es aus diesem einen Wort?

Das Wort:

```
Tischreservierungsbestätigungsnachricht

1 Wort, 39 Buchstaben
GPT-4o-Tokens: ?
```

- 1, weil es ein Wort ist.
- **✓** 9, weil es aus längeren Stücken gebaut wird, die im Vokabular stehen.
- 39, eines pro Buchstabe.
- Keins, weil ein Modell Wörter weglässt, die es nie gesehen hat.

**Warum:** GPT-4o hat für dieses seltene Wort kein einzelnes Token, also baut sein Tokenizer es aus 9 Stücken, die er kennt: T, isch, reserv, ierungs, best, ät, igungs, nach, richt. Die Antwort ist nicht 1, weil nur häufige Wörter ein eigenes Token bekommen. Sie ist nicht 39, weil Buchstabenfolgen, die oft vorkommen, als ein Stück gespeichert sind. Und nichts wird weggelassen: Jeder Text lässt sich aus Stücken bauen, bis hinunter zu einzelnen Bytes. Andere Familien zerlegen es anders: Mistral v3 braucht 13 Tokens.

## Was du jetzt kannst

- Erklären, was ein Tokenizer, ein Token und ein Vokabular sind.
- Sagen, warum derselbe Text in zwei Modellfamilien eine unterschiedliche Zahl von Tokens sein kann.
- Erklären, warum Preise und Kontextfenster von der Zahl der Tokens abhängen und warum andere Sprachen, Code und Emoji oft mehr kosten.

---

[Zurück zu Modelle verstehen](README.md) · [Weiter: Das nächste Token wählen](03-next-token.md)
