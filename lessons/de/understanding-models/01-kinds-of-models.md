# Arten von Modellen

Modelle verstehen · Kapitel 1 von 8 · 14 Minuten

Chat-, Reasoning-, multimodale, Embedding-, Sprach-, Bild-, Guard- und kleine Modelle sowie Reranker erledigen verschiedene Aufgaben, und ein Agent nutzt oft mehrere davon.

## Ziele

- Besetze den ersten Platz des Sprachagenten: Mach aus der Stimme des Anrufers Text.
- Besetze die zwei Plätze, die die Notizen finden, die die Frage beantworten.
- Besetze die letzten drei Plätze: Antwort schreiben, prüfen und aussprechen.
- Besteh den Check.

## Der Text

### Verschiedene Modelle, verschiedene Aufgaben

Ein Modell ist ein Programm, das Muster aus Beispielen gelernt hat. Modelle lernen verschiedene Aufgaben. Jedes nimmt eine bestimmte Art von Eingabe auf, etwa Text, Audio oder Bilder, und gibt eine bestimmte Art von Ausgabe aus. Ein Agent verkettet oft mehrere Modelle, und jedes erledigt die Aufgabe, die es am besten kann.

### Modelle, die schreiben

Ein Chat-Modell, auch Instruct-Modell genannt, folgt Anweisungen und schreibt eine Antwort. Ein Reasoning-Modell arbeitet ein Problem Schritt für Schritt durch, bevor es antwortet: besser bei schweren Problemen, aber langsamer. Viele neue Modelle können beides, mit einer Einstellung dafür, wie viel sie nachdenken. Ein multimodales Modell liest auch Bilder, Audio oder Video.

### Modelle, die finden und sortieren

Ein Embedding-Modell macht aus einem Text eine Liste von Zahlen, sodass Texte mit ähnlicher Bedeutung ähnliche Zahlen bekommen. Eine Suche kann dann die Zahlen vergleichen und Notizen finden, die einer Frage inhaltlich nahe sind, schnell, selbst unter Tausenden. Ein Reranker liest die Frage zusammen mit jeder Notiz, die er bekommt, und setzt die beste an den Anfang. Das ist sorgfältiger, aber langsamer, deshalb sortiert er nur die wenigen Notizen, die die Suche gefunden hat.

### Modelle, die zuhören, sprechen, zeichnen und prüfen

Sprache zu Text schreibt auf, was jemand sagt. Text zu Sprache liest Text laut vor. Ein Bildmodell erzeugt ein Bild aus einer Beschreibung. Ein Guard-Modell, auch Sicherheitsklassifikator genannt, kennzeichnet Text oder Bilder als sicher oder unsicher, damit ein Agent eine schlechte Antwort stoppen kann, bevor jemand sie sieht oder hört.

### Klein, geschlossen und offen

Die Größe eines Modells wird in Parametern gezählt: den internen Werten, die es im Training angepasst hat, auch Gewichte genannt. Kleine Modelle haben einige Milliarden Parameter oder weniger, deshalb laufen sie auf einem Handy oder Laptop. Die Gewichte eines geschlossenen Modells werden nicht veröffentlicht, und die meisten geschlossenen Modelle nutzt man über einen Onlinedienst. Ein Modell mit offenen Gewichten kannst du herunterladen und auf deinem eigenen Computer ausführen.

In der Visualisierung stattest du einen Sprachagenten für die Telefonleitung einer Bibliothek aus, einen Platz nach dem anderen. Die Bibliothek ist erfunden; jedes Beispielmodell, das dir begegnet, ist echt, Stand 2026-11-03.

## Probier es aus: Statte einen Sprachagenten aus

Besetze einen Platz nach dem anderen, von oben: Lies, was der Platz aufnimmt und ausgeben muss, und wähle dann die passende Modellkarte.

https://agenticademy.org/de/paths/understanding-models/01-kinds-of-models

## Check

### Frage 1 von 3

Ein Support-Agent muss unter 5.000 Hilfeartikeln die drei finden, die am besten zur Frage eines Kunden passen. Welches Modellpaar ist für diese Aufgabe gemacht?

- Ein Bildmodell und ein Sprache-zu-Text-Modell.
- **✓** Ein Embedding-Modell, um inhaltlich nahe Artikel zu finden, dann ein Reranker, der den besten an den Anfang setzt.
- Ein Guard-Modell und ein Text-zu-Sprache-Modell.
- Ein Reranker allein, der alle 5.000 Artikel mit der Frage liest.

**Warum:** Mit Embeddings kann eine Suche Zahlen vergleichen und findet so ähnliche Artikel schnell, selbst unter Tausenden. Ein Reranker liest dann die Frage mit jedem der wenigen gefundenen Artikel und setzt den besten an den Anfang. Ein Reranker allein würde für jede Frage alle 5.000 Paare lesen, und das ist langsam. Bild-, Sprach-, Guard- und Text-zu-Sprache-Modelle erledigen andere Aufgaben: Bilder, Audio und Sicherheitslabels.

### Frage 2 von 3

Eine Außendienst-App muss auf einem Handy ohne Internetverbindung Fragen beantworten. Welche Art von Modell kann das?

- Ein geschlossenes Chat-Modell, genutzt über den Dienst seines Herstellers.
- **✓** Ein kleines Modell mit offenen Gewichten, auf das Handy heruntergeladen.
- Ein Reasoning-Modell auf einem Server.
- Ein Embedding-Modell allein.

**Warum:** Ohne Verbindung muss das Modell auf dem Handy selbst laufen. Ein kleines Modell mit offenen Gewichten kann das: Du lädst es einmal herunter, und es läuft ohne Internet. Ein geschlossenes Chat-Modell, das über den Dienst seines Herstellers genutzt wird, und ein Reasoning-Modell auf einem Server brauchen beide eine Verbindung, um erreichbar zu sein. Ein Embedding-Modell gibt Zahlen aus, keine Antworten.

### Frage 3 von 3

Ein Anrufer stellt einem Sprachagenten eine leichte Frage und wartet in der Leitung. Warum passt ein Chat-Modell besser in den Antwort-Platz als ein Reasoning-Modell?

- Ein Reasoning-Modell kann keinen Text lesen.
- **✓** Ein Reasoning-Modell arbeitet Schritte durch, bevor es antwortet, also wartet der Anrufer länger auf eine leichte Antwort.
- Ein Chat-Modell hat immer recht.
- Ein Reasoning-Modell gibt Audio aus, keinen Text.

**Warum:** Beide nehmen Text auf und geben Text aus. Ein Reasoning-Modell denkt eine Weile nach, bevor es antwortet, was bei schweren Problemen hilft, den Anrufer bei einer leichten Frage aber in Stille warten lässt. Kein Modell hat immer recht, und Text in Audio umzuwandeln ist die Aufgabe von Text zu Sprache.

## Was du jetzt kannst

- Zehn Arten von Modellen nennen und sagen, was jede aufnimmt und ausgibt.
- Für jeden Schritt eines einfachen Agenten eine Art von Modell wählen und sagen, warum eine andere Art nicht passen würde.
- Für jede Art ein geschlossenes und ein offenes Beispiel nennen, aus einer datierten Momentaufnahme.

---

[Zurück zu Modelle verstehen](README.md) · [Weiter: Tokens](02-tokens.md)
