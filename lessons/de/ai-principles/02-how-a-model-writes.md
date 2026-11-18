# Wie ein Modell schreibt

KI-Grundlagen · Kapitel 2 von 8 · 14 Minuten

Ein Sprachmodell schreibt ein Token nach dem anderen, indem es aus einer Liste von Wahrscheinlichkeiten zieht, die Temperatur ändert, wie es zieht, und nichts in diesem Schritt prüft, ob die Wörter stimmen.

## Ziele

- Lies die Wahrscheinlichkeiten für einen Prompt und lass das Modell das wahrscheinlichste Wort nehmen.
- Wähl die nächsten fünf Wörter selbst.
- Schreib bei niedriger und bei hoher Temperatur und sieh zu, wie der Text abdriftet.
- Besteh den Check.

## Der Text

### Ein Stück nach dem anderen

Im letzten Kapitel hat ein Klassifikator aus Beispielen gelernt, Punkte in Klassen einzuteilen. Ein Sprachmodell, die Art von KI hinter Chat-Assistenten, hat aus einer riesigen Menge Text gelernt, eine einzige Sache zu tun: zu einem Text sagen, was wahrscheinlich als Nächstes kommt. Es schreibt, indem es diesen Schritt wiederholt. Es berechnet eine Wahrscheinlichkeit für jedes mögliche nächste Stück, wählt eines, hängt es an den Text an und beginnt von vorn.

### Tokens und Wahrscheinlichkeiten

Die Stücke heißen Tokens. Ein Token ist ein kurzes Stück Text: ein Wort, ein Teil eines Worts oder ein Satzzeichen. Eine Wahrscheinlichkeit ist eine Zahl von 0 % bis 100 %, und die Wahrscheinlichkeiten aller möglichen nächsten Tokens ergeben zusammen 100 %. Nach „Das Café öffnet um 8 Uhr am“ bekommt das Token „Morgen“ eine hohe Wahrscheinlichkeit und „Mond“ eine winzige. Der Text, mit dem du anfängst, heißt Prompt.

### Temperatur

Das Modell zieht das nächste Token zufällig, gewichtet nach den Wahrscheinlichkeiten, sodass wahrscheinliche Tokens am häufigsten vorkommen. Die Temperatur ist eine Einstellung, die die Wahrscheinlichkeiten vor dem Ziehen umformt. Nahe 0 gewinnt fast immer das wahrscheinlichste Token, und derselbe Prompt ergibt jedes Mal fast denselben Text. Weiter oben werden unwahrscheinliche Tokens öfter gewählt: Der Text variiert stärker und driftet ab einem gewissen Punkt in Unsinn ab.

### Flüssig heißt nicht wahr

Jedes Token wird gewählt, weil es wahrscheinlich als Nächstes kommt, nicht weil es wahr ist. Nichts in diesem Schritt prüft eine Tatsache. Ein Modell kann einen flüssigen Satz mit einem falschen Datum, Preis oder Namen schreiben, und eine niedrige Temperatur behebt das nicht: Sie wählt nur die wahrscheinlichsten Wörter. Zu prüfen, was es schreibt, bleibt deine Aufgabe, und ein späteres Kapitel zeigt, wie.

### Das Modell auf dieser Seite

Der Explorer unten führt ein winziges Modell aus, das dein Browser beim Öffnen der Seite trainiert, mit zehn Fabeln von Äsop. Seine Tokens sind ganze Wörter und Satzzeichen. Es schaut nur auf die letzten zwei und zählt, was in den Fabeln danach kam. Die Modelle hinter Chat-Assistenten nutzen Tokens, die auch Teile von Wörtern sein können, berücksichtigen viel mehr vom Text und haben Milliarden von Parametern, die Zahlen, die sie beim Lernen anpassen. Der Schritt des Auswählens funktioniert genauso.

## Probier es aus: Explorer für das nächste Wort

Lies die Wahrscheinlichkeiten für einen Prompt, wähl selbst Wörter, verschieb dann die Temperatur und sieh zu, wie der Text abdriftet.

https://agenticademy.org/de/paths/ai-principles/02-how-a-model-writes

## Check

### Frage 1 von 3

Du tippst „Unser Zug an die Küste fährt um“, und ein Modell setzt den Text fort. Was macht es?

- Es schlägt den Fahrplan nach und kopiert die Uhrzeit.
- **✓** Es gibt jedem möglichen nächsten Token eine Wahrscheinlichkeit, zieht eins, hängt es an und macht dann dasselbe noch einmal.
- Es schreibt die ganze Antwort auf einmal und prüft sie dann an den Fakten.
- Es nimmt immer das überraschendste Token, damit der Text interessant bleibt.

**Warum:** Ein Sprachmodell schreibt ein Token nach dem anderen und zieht jedes aus Wahrscheinlichkeiten. Es schlägt nichts nach, außer es ist mit einem Tool verbunden, es schreibt nicht alles auf einmal und prüft keine Fakten, und wahrscheinliche Tokens werden am häufigsten gewählt, nicht überraschende.

### Frage 2 von 3

Du stellst einem Modell zweimal dieselbe Frage bei hoher Temperatur. Womit solltest du rechnen?

- Beide Male genau dieselbe Antwort.
- **✓** Antworten, die sich unterscheiden können, weil jedes Token zufällig aus umgeformten Wahrscheinlichkeiten gezogen wird.
- Eine Fehlermeldung, weil eine hohe Temperatur nicht erlaubt ist.
- Genauere Antworten, weil das Modell mehr Möglichkeiten ausprobiert.

**Warum:** Eine hohe Temperatur gleicht die Wahrscheinlichkeiten an, sodass jede Ziehung anders ausgehen kann und sich zwei Antworten unterscheiden können. Nahe 0 wären sie fast gleich. Eine hohe Temperatur ist eine erlaubte Einstellung, und sie macht Antworten nicht genauer: Sie macht nur unwahrscheinliche Tokens häufiger.

### Frage 3 von 3

Ein Modell schreibt flüssig und ohne Einschränkung: „Das Café in der Elm Street öffnet sonntags um 7:30 Uhr.“ Was sagt dir die flüssige Formulierung über die Uhrzeit?

- Sie stimmt, sonst würde das Modell sie nicht so flüssig schreiben.
- Sie stimmt, wenn die Temperatur niedrig war.
- **✓** Nichts Sicheres: Die Wörter wurden gewählt, weil sie wahrscheinlich waren, also prüf die Uhrzeit beim Café.
- Sie ist falsch, weil Modelle Uhrzeiten immer erfinden.

**Warum:** Eine flüssige Formulierung zeigt, dass die Tokens wahrscheinlich waren, nicht dass sie wahr sind. Eine niedrige Temperatur lässt das Modell nur die wahrscheinlichsten Tokens wählen, die trotzdem falsch sein können, und Modelle liegen auch nicht immer falsch. Prüf Fakten wie Öffnungszeiten an einer Quelle, der du vertraust.

## Was du jetzt kannst

- Erklären, dass ein Sprachmodell ein Token nach dem anderen schreibt und jedes aus Wahrscheinlichkeiten zieht.
- Sagen, was die Temperatur verändert und was nicht.
- Erklären, warum eine flüssige, selbstsichere Antwort trotzdem falsch sein kann.

---

[Zurück zu KI-Grundlagen](README.md) · [Weiter: Hier stark, dort schwach](03-strong-here-weak-there.md)
