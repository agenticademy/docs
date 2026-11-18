# Hier stark, dort schwach

KI-Grundlagen · Kapitel 3 von 8 · 15 Minuten

KI erledigt manche schweren Aufgaben gut und scheitert an leicht wirkenden, kann Falsches mit Überzeugung behaupten und ist nützlicher, wenn sie sagt, dass sie etwas nicht weiß.

## Ziele

- Beantworte sechs Fragen oder sag „Ich weiß es nicht“ und bewerte sie dann auf zwei Arten.
- Sortiere sechs Alltagsaufgaben auf eine datierte Karte, die zeigt, wo KI gut ist und wo sie scheitert.
- Besteh den Check.

## Der Text

### Ungleichmäßig, und nicht nach Schwierigkeit

In Kapitel 2 hast du gesehen, wie ein Modell schreibt, indem es wahrscheinliche nächste Textstücke wählt, gelernt aus den Beispielen, mit denen es trainiert wurde. Was es gut kann, hängt von diesen Beispielen ab, nicht davon, was Menschen schwer finden. Es kann ein Schild in Sekunden in drei Sprachen übersetzen und dann die Buchstaben in einem Wort falsch zählen, weil es Wortstücke liest statt einzelner Buchstaben.

Forschende nennen das eine gezackte Grenze: Die Linie zwischen dem, was KI gut kann, und dem, was sie schlecht kann, verläuft im Zickzack. Der Begriff stammt aus einer Studie von 2023, in der Consultants KI bei realistischen Aufgaben einsetzten (Dell'Acqua und Kollegen). Bei Aufgaben innerhalb der Grenze wurde ihre Arbeit besser. Bei einer Aufgabe knapp außerhalb lagen diejenigen mit KI seltener richtig als diejenigen, die ohne KI arbeiteten. Die Grenze verschiebt sich auch, wenn sich Modelle ändern, deshalb braucht jede Karte von ihr ein Datum.

### Selbstsicher und falsch

Ein Modell gibt immer eine Antwort, und seine Worte klingen genauso sicher, wenn es falschliegt. Etwas Falsches in selbstsicherem Ton zu schreiben, etwa ein Zitat aus einem Buch, das es nicht gibt, nennt man Konfabulation; viele nennen es Halluzination. Kalibriert sein heißt, dass deine Sicherheit dazu passt, wie oft du richtigliegst: Wenn du sagst, dass du sicher bist, bist du es fast immer.

### Warum sich Raten bei den meisten Tests lohnt

Sich enthalten heißt, „Ich weiß es nicht“ zu sagen, statt zu antworten. Ob sich das lohnt, hängt von der Bewertungsregel ab, also davon, wie ein Test Antworten in Punkte umrechnet. Wenn nur richtige Antworten zählen, kann Raten nur helfen, also schlägt jemand, der immer antwortet, jemanden, der Zweifel zugibt. Ein Paper von Forschenden bei OpenAI aus dem Jahr 2025 argumentiert, dass die meisten Tests, mit denen KI-Modelle in Ranglisten eingeordnet werden, auf diese Weise bewerten, weshalb ein ratendes Modell vor einem landet, das Unsicherheit zugibt. Die Lösung, die es vorschlägt: Eine falsche Antwort soll mehr kosten als „Ich weiß es nicht“.

### In der Visualisierung

Du beantwortest sechs Fragen; einige der Antworten könnte niemand aus dieser Seite wissen. Deine Antworten werden auf beide Arten bewertet, neben zwei simulierten Spielern. Dann sortierst du sechs Alltagsaufgaben auf eine datierte Karte: meist in Ordnung, genau prüfen oder nicht darauf verlassen.

## Probier es aus: Antworten oder sich enthalten, dann die Aufgaben einordnen

Beantworte sechs Fragen, sieh sie auf zwei Arten bewertet und sortiere dann sechs Alltagsaufgaben auf eine datierte Karte.

https://agenticademy.org/de/paths/ai-principles/03-strong-here-weak-there

## Check

### Frage 1 von 3

Ein Quiz gibt 1 Punkt für eine richtige Antwort, 0 für „Ich weiß es nicht“ und minus 1 für eine falsche. Du bist dir deiner Antwort zu etwa 30 % sicher. Was bringt im Schnitt die meisten Punkte?

- Trotzdem antworten: Raten lohnt sich bei jeder Bewertungsregel.
- **✓** „Ich weiß es nicht“ sagen: Bei 30 % Sicherheit kostet Raten im Schnitt Punkte.
- Egal: Beides bringt im Schnitt gleich viele Punkte.
- Antworten und eine längere Erklärung dazuschreiben, um mehr zu bekommen.

**Warum:** Antworten bringt im Schnitt 0,3 × 1 + 0,7 × (−1) = −0,4 Punkte, „Ich weiß es nicht“ bringt 0, also ist beides nicht gleich, und sich zu enthalten schneidet am besten ab. Raten lohnt sich nur, wenn falsche Antworten nichts kosten, und eine längere Erklärung ändert die Punkte nicht. Dasselbe gilt für ein Modell, das so bewertet wird.

### Frage 2 von 3

Ein Chat-Assistent übersetzt einen langen Brief gut ins Japanische und zählt dann die Buchstaben in einem kurzen Wort falsch. Was zeigt das?

- Er ist kaputt, also kann man keiner seiner Arbeiten trauen.
- Buchstaben zählen ist schwerer als übersetzen, auch für Menschen.
- Er hat sich bei der leichten Aufgabe keine Mühe gegeben, wie ein gelangweilter Mensch.
- **✓** Was er gut kann, folgt nicht dem, was Menschen schwer finden.

**Warum:** Das ist die gezackte Grenze: Ein Modell kann eine Aufgabe, die für Menschen schwer ist, gut erledigen und bei einer patzen, die ein Kind schafft, weil es Wortstücke liest, keine Buchstaben. Beurteile also jede Art von Aufgabe für sich. Ein Patzer macht nicht seine ganze Arbeit unzuverlässig, Zählen ist für Menschen nicht schwerer, und es hat nicht geschludert: Der Patzer kommt daher, wie es Text liest.

### Frage 3 von 3

Du fragst einen Chat-Assistenten ohne Websuche, ob die Bäckerei in deiner Straße gerade geöffnet hat. Er antwortet: „Ja, sie hat heute bis 18 Uhr geöffnet.“ Was solltest du tun?

- **✓** Es als Vermutung behandeln und auf der Seite der Bäckerei selbst nachsehen oder anrufen.
- Ihm vertrauen: Er klingt sicher, also muss er es wissen.
- Noch einmal fragen: Wenn er zweimal dasselbe sagt, stimmt es.
- Ihm vertrauen, weil die Antwort kurz und konkret ist.

**Warum:** Er kann die heutigen Öffnungszeiten nicht sehen, also kann eine sichere, konkrete Antwort trotzdem Konfabulation sein: Die Worte eines Modells klingen gleich, ob es etwas weiß oder nicht. Noch einmal fragen bringt nur eine weitere Vermutung, die passen kann oder nicht. Prüf eine Quelle, die es wirklich weiß.

## Was du jetzt kannst

- Erklären, warum KI eine schwere Aufgabe gut erledigen und an einer leicht wirkenden scheitern kann.
- Sagen, warum eine selbstsichere Antwort trotzdem falsch sein kann und warum es Raten belohnt, nur richtige Antworten zu zählen.
- Bei einer Alltagsaufgabe entscheiden, ob das Ergebnis der KI meist in Ordnung ist, geprüft werden muss oder ob man sich nicht darauf verlassen sollte.

---

[Zurück zu KI-Grundlagen](README.md) · [Weiter: Was du abgibst](04-what-to-hand-over.md)
