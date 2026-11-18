# Offene und geschlossene Modelle

Modelle verstehen · Kapitel 6 von 8 · 15 Minuten

Manche Modelle lassen sich nur über den Dienst ihres Herstellers nutzen, manche teilen ihre Gewichte unter Lizenzen von streng bis permissiv, und einige wenige sind Open Source, mit ihren Daten und ihrem Code.

## Ziele

- Wähle ein Ziel, etwa offline arbeiten oder eine kommerzielle App veröffentlichen.
- Prüfe die Modelle Ebene für Ebene und sieh, welche für dieses Ziel infrage kommen.
- Öffne eine Lizenz und lies ihre wichtigsten Bedingungen.
- Besteh den Check.

## Der Text

### Geschlossen: ein Dienst, den du aufrufst

In Kapitel 4 haben offene Modelle ihre Gewichte veröffentlicht, die Zahlen, die ein Modell gelernt hat, und geschlossene nicht. Ein geschlossenes Modell läuft auf den Computern seines Herstellers. Du erreichst es über die App des Herstellers oder seine API, einen Dienst, an den dein Programm Text schickt, und zahlst pro Token nach den Nutzungsbedingungen des Herstellers. Claude, GPT, Gemini und Grok funktionieren so.

### Offene Gewichte, unter einer Lizenz

Bei einem Modell mit offenen Gewichten kannst du die Gewichte herunterladen. Du kannst es auf deinem eigenen Computer ohne Internetverbindung ausführen und per Fine-Tuning anpassen: es mit deinen eigenen Beispielen weitertrainieren. Zu den Gewichten gehört eine Lizenz, der Rechtstext, der sagt, was du damit tun darfst.

Eine permissive Lizenz wie Apache-2.0 oder MIT erlaubt fast jede Nutzung, auch kommerziell, solange du ihren Hinweis beibehältst. Eine restriktive Lizenz fügt Bedingungen hinzu, etwa eine eigene Vereinbarung ab einer Zahl von Nutzern oder einem Umsatz, einen Namen, den du zeigen musst, oder Nutzungen, die nicht erlaubt sind. Gemma 4, gpt-oss und DeepSeek-V4.1-Flash haben permissive Lizenzen; Llama 4 und Kimi K3 haben restriktive.

### Open Source

Offene Gewichte sind nicht Open Source. Die Open Source Initiative (OSI), die die Definition von Open-Source-Software pflegt, hat die Open Source AI Definition 1.0 geschrieben. Sie verlangt drei Dinge unter offenen Bedingungen: die Gewichte, den Code, der das Modell trainiert und ausführt, und Dateninformationen, genug über die Trainingsdaten, damit eine fachkundige Person ein ähnliches Modell bauen kann. Wenige Modelle erfüllen sie. Olmo 3, das du in Kapitel 4 verfolgt hast, veröffentlicht alle drei.

### Sie bekommen und ausführen

Offene Modelle werden auf Hubs wie Hugging Face geteilt, einer Website, die die Dateien, die Model Card und die Lizenz jedes Modells bereitstellt. Manche sind zugangsbeschränkt: Du akzeptierst die Lizenz und wartest manchmal auf die Freigabe des Herstellers, bevor der Download beginnt. Auf deinem Computer führen Apps sie aus Dateien aus, die für sie gemacht sind: llama.cpp und Ollama lesen GGUF-Dateien, MLX führt Modelle auf Macs mit Apple silicon aus, und ein Browser mit WebGPU kann kleine ausführen, wie in Kapitel 3. Ob eines passt, ist die Frage von Kapitel 5.

### In der Visualisierung

Du wählst ein Ziel und prüfst echte Modelle Ebene für Ebene, von geschlossen bis Open Source. Dann öffnest du eine Lizenz. Die Fakten stammen von den Seiten der Modelle und den Lizenzen, in einer Momentaufnahme vom 2026-11-03. Sie ist eine Zusammenfassung zum Lernen, keine Rechtsberatung.

## Probier es aus: Lizenzmatrix

Wähle ein Ziel, prüfe die Modelle Ebene für Ebene von geschlossen bis Open Source, öffne dann eine Lizenz und lies ihre wichtigsten Bedingungen.

https://agenticademy.org/de/paths/understanding-models/06-open-and-closed

## Check

### Frage 1 von 3

Du brauchst ein Modell auf einem Laptop ohne Internetverbindung. Welche Art kann das?

- Ein geschlossenes Modell, über seine API vom Laptop aus genutzt.
- **✓** Ein Modell mit offenen Gewichten oder ein Open-Source-Modell, vorher auf den Laptop heruntergeladen.
- Jedes beliebige Modell, solange du einen API-Schlüssel dafür hast.
- Keines: Jedes Modell braucht zum Antworten das Internet.

**Warum:** Offline arbeiten heißt, dass die Gewichte auf dem Laptop sein müssen, und nur Modelle mit offenen Gewichten und Open-Source-Modelle lassen dich sie herunterladen. Ein geschlossenes Modell läuft auf den Servern seines Herstellers, also braucht seine API eine Verbindung, mit oder ohne Schlüssel. Ein heruntergeladenes offenes Modell braucht keine, wenn der Laptop den Speicher dafür hat.

### Frage 2 von 3

Die Gewichte eines Modells sind unter Apache-2.0 veröffentlicht, aber über seine Trainingsdaten wird nichts gesagt. Auf welcher Ebene ist es?

- Geschlossen, weil über seine Trainingsdaten nichts gesagt wird.
- Offene Gewichte, restriktive Lizenz.
- Open Source, nach der Definition der OSI.
- **✓** Offene Gewichte, permissive Lizenz.

**Warum:** Geschlossen heißt, dass die Gewichte beim Hersteller bleiben, und diese sind veröffentlicht, egal was über die Daten gesagt wird. Apache-2.0 erlaubt kommerzielle Nutzung ohne Grenzen bei Nutzern oder Umsatz, also ist die Lizenz permissiv, nicht restriktiv. Open Source ist es nicht: Die Definition der OSI verlangt auch Dateninformationen und den Trainingscode, und eine permissive Lizenz allein liefert sie nicht.

### Frage 3 von 3

Die Lizenz von Llama 4 sagt, dass du ab mehr als 700 Millionen monatlich aktiven Nutzern eine eigene Lizenz von Meta brauchst. Ein Start-up mit 20.000 Nutzern baut eine kostenpflichtige App auf Llama 4. Was bedeutet diese Bedingung für das Start-up?

- Es darf Llama 4 nicht in einer kostenpflichtigen App nutzen, bis Meta ihm eine eigene Lizenz gibt.
- Es muss Meta pro Token bezahlen, um Llama 4 zu nutzen, wie bei einem geschlossenen Modell.
- **✓** Die Nutzergrenze gilt nicht für es, aber die anderen Bedingungen der Lizenz gelten bereits.
- Nichts in der Lizenz gilt für es, bis es 700 Millionen monatliche Nutzer hat.

**Warum:** Die Grenze beginnt über 700 Millionen monatlich aktiven Nutzern, also darf ein kleines Start-up ohne eigene Lizenz eine kostenpflichtige App bauen. Die anderen Bedingungen gelten ab dem ersten Nutzer: Die Lizenz von Llama 4 verlangt, „Built with Llama“ zu zeigen und Metas Nutzungsrichtlinie zu befolgen. Offene Gewichte werden nicht pro Token bezahlt: Du führst sie selbst aus.

## Was du jetzt kannst

- Geschlossene Modelle, offene Gewichte unter restriktiven oder permissiven Lizenzen und Open Source auseinanderhalten.
- Sagen, welche Arten von Modellen zu einem Ziel passen: eine kommerzielle App, Offline-Nutzung, Fine-Tuning oder bekannte Trainingsdaten.
- Die wichtigsten Bedingungen einer Lizenz lesen, etwa Grenzen bei Nutzern und Umsatz, bevor du auf einem Modell aufbaust.
- Nennen, wo offene Modelle geteilt werden und welche Apps sie auf deinem eigenen Computer ausführen.

---

[Zurück zu Modelle verstehen](README.md) · [Weiter: Modelle vergleichen](07-comparing-models.md)
