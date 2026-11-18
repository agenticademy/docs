# Rechenleistung einsetzen

Agenten für Fortgeschrittene · Kapitel 8 von 10 · 20 Minuten

Evaluator-Optimizer-Schleifen, Reflexion, Best-of-n und Baumsuche geben mehr Modellaufrufe aus, um eine bessere Antwort zu bekommen, und jede davon braucht ein Budget.

## Ziele

- Führe eine Schleife aus Generator und Richter mit einem Bewertungsraster und einem Rundenlimit aus.
- Erzeuge mehrere Antworten und wähle eine per Abstimmung.
- Durchsuche einen kleinen Baum von Plänen mit einem Expansionsbudget.
- Besteh die Prüfung.

## Der Text

### Mehr Aufrufe für eine bessere Antwort

Ein Modellaufruf ergibt eine Antwort. Mehr Aufrufe können eine bessere ergeben: Ein Richter bewertet Entwürfe, mehrere Samples werden verglichen, oder eine Suche schaut voraus. Jedes Muster vervielfacht die Aufrufe, die Kosten und die Wartezeit, also braucht jedes ein Budget, das es stoppt.

### Ein Generator und ein Richter

Eine Evaluator-Optimizer-Schleife hat zwei Rollen. Der Generator schreibt einen Entwurf. Der Richter ist ein eigener Modellaufruf mit eigenem Prompt: Er bewertet den Entwurf nach einem Bewertungsraster, einer kurzen Liste von Kriterien, die jeweils auf einer Skala bewertet werden, und schreibt Hinweise für den nächsten Entwurf. Ein Schwellenwert legt fest, wann ein Entwurf besteht, etwa jedes Kriterium mit 4 oder mehr von 5. Ein Rundenlimit begrenzt die Schleife: Ist es erreicht, gibt die Schleife den bisher besten Entwurf zurück, als nicht bestanden markiert.

Reflexion, oder Self-Refine, ist dieselbe Schleife, in der das Modell seinen eigenen Entwurf kritisiert. Ein eigener Richter lässt sich leichter streng halten und testen. Wie in Agentensysteme, Kapitel 8: Vergleiche seine Bewertungen mit denen von Menschen, bevor du ihm vertraust.

### Best-of-n und Abstimmung

Best-of-n fragt n Antworten auf denselben Prompt ab, gesampelt mit einer Temperatur über 0, damit sie sich unterscheiden, und behält eine: Ein Richter wählt die beste, oder eine Abstimmung behält die häufigste Endantwort. Abstimmen über Endantworten heißt Self-Consistency; es passt zu Antworten, die sich exakt vergleichen lassen, etwa ein Betrag. In LangGraph sind die Samples n Sends in einem Superstep: n Aufrufe, die gleichzeitig laufen, also wartest du nur auf den langsamsten.

### Baumsuche

Eine Baumsuche plant Schritte im Voraus. Sie erweitert einen Schritt um mögliche nächste Schritte, gibt jedem eine Wertschätzung, wie vielversprechend er aussieht, und erweitert weiter den vielversprechendsten offenen Schritt. Language Agent Tree Search (LATS, 2023) macht das mit Monte-Carlo-Baumsuche und nutzt das Modell, um Schritte vorzuschlagen, Werte zu schätzen und über Fehlschläge zu reflektieren. Ein Expansionsbudget begrenzt, wie viele Schritte sie erweitert. Das Lab führt eine kleinere Best-First-Suche über zwei Ebenen aus, mit simulierten Werten.

### Wann es sich nicht lohnt

Zusätzliche Aufrufe lohnen sich, wenn eine bessere Antwort zählt und etwas Besseres von Schlechterem unterscheiden kann: ein Bewertungsraster, eine exakte Antwort zum Abstimmen, ein Wert zum Schätzen. Wenn schon ein Aufruf besteht oder nichts die Qualität prüfen kann, kosten sie nur Geld und Zeit. Miss es: Führe deine Evaluationsfälle mit und ohne das Muster aus.

## Probier es aus: Drei Wege, mehr Aufrufe auszugeben

Haldens Ticket T-48213, auf drei Wegen: Eine Richter-Schleife verfeinert die Antwort, eine Abstimmung wählt den Erstattungsbetrag, und eine Baumsuche plant die Schritte. Jeder stoppt an seinem Budget.

https://agenticademy.org/de/paths/advanced-agents/08-spending-compute

## Check

Was gibt dieser Lauf zurück, und nach wie vielen Modellaufrufen (einer pro Entwurf, einer pro Bewertung)?

Code:

```
const CAP = 3;
const PASS = 4; // per criterion, of 5
const scores = (r) =>
  Object.values(r.scores);
const passes = (r) =>
  scores(r).every((s) => s >= PASS);
const total = (r) =>
  scores(r).reduce((a, b) => a + b);

function review(state) {
  const last = state.rounds.at(-1);
  if (passes(last)) return 'finish';
  if (state.rounds.length >= CAP)
    return 'finish';
  return 'generate';
}

function finish(state) {
  const last = state.rounds.at(-1);
  const best = passes(last) ? last
    : state.rounds.reduce((b, r) =>
        (total(r) > total(b) ? r : b));
  return {
    answer: best.draft,
    passed: passes(best),
  };
}

// The judge's scores, round by round:
// 1: accurate 3, complete 2, tone 4
// 2: accurate 4, complete 4, tone 3
// 3: accurate 4, complete 3, tone 3
```

- Entwurf 3, als bestanden markiert, nach 6 Aufrufen.
- **✓** Entwurf 2, als nicht bestanden markiert, nach 6 Aufrufen.
- Entwurf 3, als nicht bestanden markiert, nach 6 Aufrufen.
- Noch nichts: Eine vierte Runde läuft, weil kein Entwurf bestanden hat.

**Warum:** Keine Runde besteht: Runde 1 hat complete 2, Runde 2 hat tone 3, Runde 3 hat complete 3 und tone 3. Also schickt review die Schleife zurück, bis rounds.length nach Runde 3 CAP erreicht: drei Entwürfe und drei Bewertungen, 6 Aufrufe, und keine vierte Runde. finish behält dann die beste Summe, Entwurf 2 mit 11 vor Entwurf 3 mit 10, weil eine Überarbeitung schlechter werden kann; der letzte Entwurf ist nicht immer der beste. passed ist false, also kann der Aufrufer den Entwurf markieren oder an einen Menschen geben.

## Was du jetzt kannst

- Eine Evaluator-Optimizer-Schleife mit eigenem Richter, Bewertungsraster, Bestehensschwelle und Rundenlimit bauen.
- Best-of-n-Samples als Sends in einem Superstep ausführen und die Antwort behalten, auf die sich die meisten einigen.
- Erklären, wie eine Baumsuche mit einem Expansionsbudget erweitert, bewertet und Werte zurückführt.
- Entscheiden, wann zusätzliche Aufrufe ihre Kosten wert sind, und es mit deinen Evaluationsfällen messen.

---

[Zurück zu Agenten für Fortgeschrittene](README.md) · [Weiter: Ein Deep-Agent-Harness](09-deep-agent-harness.md)
