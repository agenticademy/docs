# Guardrails

Bau deine ersten Agenten · Kapitel 8 von 8 · 22 Minuten

Prüfungen im Code rund um das Modell: Eingabe- und Ausgabeprüfungen, Tool-Allow-Lists und Schrittlimits, in Schichten.

## Ziele

- Schalte jede Guardrail ein und sieh, was sie stoppt.
- Stopp alle vier Angriffe in einem Lauf.
- Sag voraus, was das Schrittlimit tut.

## Der Text

### Regeln, aus denen sich das Modell nicht herausreden kann

Eine Guardrail ist eine Prüfung in deinem Code, außerhalb des Modells. Ein Prompt kann das Modell bitten, eine Regel zu befolgen, und eine geschickte Nachricht kann es von dieser Regel abbringen. Code, der einen Aufruf verweigert, lässt sich von nichts abbringen.

### Vier Arten von Prüfung

Eingabeprüfungen testen die Nachricht des Nutzers, bevor das Modell sie liest, und verweigern oder markieren sie. Ausgabeprüfungen testen die Antwort, bevor jemand sie sieht: Zum Beispiel muss jeder Betrag, den sie nennt, in einem Tool-Ergebnis vorkommen, damit das Modell keinen erfinden kann.

Allow-Lists begrenzen, was passieren kann: welche Tools laufen dürfen, welche Hosts sie erreichen dürfen, welche Beträge sie bewegen dürfen. Sie verweigern standardmäßig, also wird ein Tool oder Argument, das niemand eingetragen hat, abgelehnt, selbst wenn eine in einem Ticket versteckte Anweisung danach verlangt. Leg die Allow-List um den Tool-Knoten, damit sie verweigert, bevor das Tool läuft.

Schrittlimits stoppen außer Kontrolle geratene Loops. recursionLimit in der Config des Laufs begrenzt die Supersteps, und LangGraph wirft GraphRecursionError, wenn es erreicht ist. Fang den Fehler über err.name ab und mach daraus einen klaren Stopp. Ein Budget für Tokens oder Modellaufrufe funktioniert genauso.

### In Schichten

Keine einzelne Prüfung fängt alles ab. Die Eingabeprüfung sieht nie Text, der in einem Tool-Ergebnis ankommt; die Allow-List liest keine Antworten. Also stapelst du sie, und wenn eine Schicht etwas verpasst, kann die nächste den Angriff noch stoppen.

Eigene Sicherheitsmodelle sind eine weitere Schicht: Klassifikatoren wie NVIDIAs Nemotron-Content-Safety-Modelle und der Llama Nemotron Safety Guard lesen eine Nachricht und stufen sie als safe oder unsafe ein. Sie fangen ab, was ein Muster verpasst, aber sie sind auch Modelle, also behalte die Prüfungen im Code bei.

## Probier es aus: Vier Guardrails, vier Angriffe

Starte vier Angriffe auf einen Agenten ohne Guardrails. Schalte dann pro Angriff eine Guardrail ein und sieh, was sie stoppt. Zum Schluss stoppt ein einziger Lauf alle vier.

https://agenticademy.org/de/paths/first-agents/08-guardrails

## Check

Das geskriptete Modell fordert immer noch einen Tool-Call an. Was passiert?

Code:

```
await agent.invoke(
  { messages: [new HumanMessage("Check ticket T-48213")] },
  { recursionLimit: 6 },
);
```

- Es läuft ewig.
- **✓** Es wirft GraphRecursionError nach 6 Schritten.
- Es gibt nach 6 Schritten die letzte Nachricht zurück, ohne Fehler.
- recursionLimit gilt nur für Subgraphen.

**Warum:** recursionLimit begrenzt die Zahl der Supersteps. Ist es erreicht, wirft LangGraph GraphRecursionError, das dein Code abfängt und in einen klaren Stopp verwandelt.

## Was du jetzt kannst

- Eingabe- und Ausgabeprüfungen rund um ein Modell schreiben.
- Einen Tool-Knoten mit einer Allow-List umgeben, die standardmäßig verweigert.
- Die Schritte eines Agenten begrenzen und GraphRecursionError behandeln.
- Erklären, warum Guardrails in Schichten kommen.

---

[Zurück zu Bau deine ersten Agenten](README.md)
