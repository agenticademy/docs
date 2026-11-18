# Modelle verstehen

Einstieg · Ohne Code · 8 Kapitel · 120 Min.

Für alle, die wissen wollen, was in den Modellen hinter KI-Werkzeugen steckt. Du siehst, wie Text zu Tokens wird, wie ein Modell das nächste wählt, wie Modelle entstehen, wie groß sie sind und was ihre Lizenzen erlauben, und dann wählst du ein Modell für einen Agenten. Die Beispiele stellen geschlossene und offene Modelle nebeneinander, und jede Zahl stammt aus einer datierten Momentaufnahme.

## Bevor du anfängst

Nichts. Die ersten beiden Kapitel von KI-Grundlagen helfen.

## Am Ende kannst du

- Die Arten von Modellen nennen, die ein Agent nutzt, und was jede davon tut.
- Tokens erklären, wie ein Modell das nächste wählt und was die Temperatur ändert.
- Verfolgen, wie ein Modell entsteht, von den Daten und dem Pre-Training bis zur quantisierten Datei.
- Geschlossene Modelle, offene Gewichte und Open Source unterscheiden und die wichtigsten Bedingungen einer Lizenz lesen.
- Eine Rangliste mit Vorsicht lesen und ein Modell für einen Agenten nach Fähigkeit, Kosten und Richtlinien wählen.

## Kapitel

1. [Arten von Modellen](01-kinds-of-models.md): Chat-, Reasoning-, multimodale, Embedding-, Sprach-, Bild-, Guard- und kleine Modelle sowie Reranker erledigen verschiedene Aufgaben, und ein Agent nutzt oft mehrere davon.
2. [Tokens](02-tokens.md): Modelle lesen und schreiben Text als Tokens, jede Modellfamilie zerlegt Text auf ihre eigene Weise, und sowohl Preise als auch Kontextgrenzen werden in Tokens gezählt.
3. [Das nächste Token wählen](03-next-token.md): Ein Modell gibt jedem möglichen nächsten Token einen Score, macht aus den Scores Wahrscheinlichkeiten und zieht eins zufällig; Temperatur, Top-k und Top-p verändern die Ziehung, und Basis-, Instruct- und Reasoning-Modelle sind darauf trainiert, unterschiedlich zu schreiben.
4. [Woher Modelle kommen](04-where-models-come-from.md): Ein Modell entsteht in Stufen, von den Daten und dem Pre-Training über Fine-Tuning, Feedback und Destillation bis zur Quantisierung, und bei offenen Modellen hinterlassen viele Stufen eine Datei, die du dir ansehen kannst.
5. [Größe und Speicher](05-size-and-memory.md): Den Speicher, den ein Modell braucht, bestimmen seine Parameter und die Bits, mit denen jeder gespeichert wird, plus Platz für seinen Kontext; ein Mixture-of-Experts-Modell nutzt für jedes Token nur wenige seiner Parameter, hält aber alle im Speicher.
6. [Offene und geschlossene Modelle](06-open-and-closed.md): Manche Modelle lassen sich nur über den Dienst ihres Herstellers nutzen, manche teilen ihre Gewichte unter Lizenzen von streng bis permissiv, und einige wenige sind Open Source, mit ihren Daten und ihrem Code.
7. [Modelle vergleichen](07-comparing-models.md): Benchmarks und Ranglisten helfen dir, Modelle zu vergleichen, aber Fehlermargen, Aufwandsstufen, Kosten, durchgesickerte Tests, ausgereizte Tests, das Testprogramm und wer den Test durchgeführt hat, ändern alle, was ein Score bedeutet.
8. [Ein Modell für einen Agenten wählen](08-choosing-for-an-agent.md): Ein Modell wählen heißt: die streichen, die an einer harten Grenze scheitern, und dann Fähigkeit, Tempo, Preis pro Aufgabe, Kontext und Zuverlässigkeit bei Tools für die Arbeit eines Agenten abwägen.

https://agenticademy.org/de/paths/understanding-models
