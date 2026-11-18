# Subgraphen und Teams

Agenten für Fortgeschrittene · Kapitel 3 von 10 · 20 Minuten

Baue Teams von Agenten aus Subgraphen, entscheide, wie sie den State teilen, lies ihre Namespaces im Stream und vergleiche Multi-Agent-Muster nach Modellaufrufen und Tokens.

## Ziele

- Führe ein Team aus Teams aus und lies die Namespaces in seinem Stream.
- Pausiere in einem Team und öffne seinen eigenen State.
- Vergleiche Subagenten mit Übergaben nach Modellaufrufen und Tokens.
- Sag voraus, was ein Subgraph streamt.

## Der Text

### Subgraphen

Ein kompilierter Graph kann ein Knoten eines anderen Graphen sein: ein Subgraph. Teilen die beiden einen State-Key, ist der kompilierte Subgraph selbst der Knoten, wie in addNode("research\_team", researchTeam). Er liest die Keys, die er mit dem Eltern-Graphen teilt, was er in sie schreibt, kommt zurück, und Keys, die nur er hat, bleiben drinnen.

Ohne gemeinsamen Key rufst du den Subgraphen aus einem Wrapper-Knoten auf, der den State des Eltern-Graphen auf die Eingabe des Subgraphen abbildet und seine Ausgabe zurück auf ein Update. Übergib { subgraphs: [team] } an addNode, damit getGraphAsync und getState ihn weiterhin finden.

### Namespaces und Streaming

Jeder Lauf eines Subgraphen speichert seine Checkpoints in einem eigenen Namespace im Thread des Eltern-Graphen, benannt nach seinem Knoten und seinem Task: research\_team:<task id>. stream(input, { streamMode: "updates", subgraphs: true }) liefert Paare [namespace, update], mit [] für den obersten Graphen. getGraphAsync({ xray: true }) zeichnet die eigenen Knoten der Subgraphen, etwa research\_team:lead.

Solange ein Lauf in einem Subgraphen pausiert, gibt getState(config, { subgraphs: true }) auch den eigenen State des Subgraphen zurück. Im Browser kann interrupt() nicht laufen, weil es AsyncLocalStorage braucht. Deshalb pausiert dieses Lab mit interruptBefore und reicht config von Hand an jeden Subgraphen weiter.

### Teams aus Teams und Übergaben

Ein Supervisor kann andere Supervisoren leiten. Im Lab routet eine Support-Leitung mit Command({ goto }) an zwei Teams, und die Leitung jedes Teams gibt Teile der Arbeit an ihre Worker. Ein Knoten in einem Subgraphen kann die Kontrolle auch an einen Knoten des Eltern-Graphen abgeben: new Command({ graph: Command.PARENT, goto: "writer", update }). Das ist eine Übergabe: Der nächste Agent übernimmt mit dem Verlauf, den du in update gelegt hast.

### Ein Muster wählen

Die JS-Doku von LangChain nennt fünf Multi-Agent-Muster. Subagenten: Ein Hauptagent ruft andere Agenten wie Tools auf und bekommt jedes Ergebnis zurück. Übergaben: Der zuständige Agent wechselt. Skills: Ein Agent lädt Spezialisten-Prompts, wenn er sie braucht. Router: Ein Schritt klassifiziert die Eingabe und schickt sie an Agenten. Eigener Workflow: dein eigener LangGraph-Graph. Kapitel 5 von Agentensysteme nannte seine transfer\_to\_<agent>-Aufrufe Übergaben, aber sein Supervisor nutzte Subagenten: Jedes Ergebnis kam zu ihm zurück.

Die Doku vergleicht sie (gelesen am 2026-11-04): Eine einmalige Anfrage braucht 4 Modellaufrufe mit Subagenten und 3 mit Übergaben; eine Frage über drei Bereiche braucht 5 Aufrufe und etwa 9K Tokens mit Subagenten, 7+ Aufrufe und etwa 14K+ mit Übergaben. Im Lab führst du einen Auftrag auf beide Arten aus und zählst.

## Probier es aus: Ein Team aus Teams

Drei Schritte: Führe ein Team aus Teams aus und lies seinen Stream, pausiere in einem Team, um seinen State zu öffnen, und führe dann denselben Auftrag als Subagenten und als Übergaben aus.

https://agenticademy.org/de/paths/advanced-agents/03-subgraphs-and-teams

## Check

Das Team teilt messages mit dem obersten Graphen; notes ist sein eigener Key. Was gibt die Schleife aus?

Code:

```
const Team = Annotation.Root({
  ...MessagesAnnotation.spec,
  notes: Annotation({ reducer: (a, b) => a.concat(b), default: () => [] }),
});
const team = new StateGraph(Team)
  .addNode("work", () => ({ notes: ["n1"], messages: [{ role: "assistant", content: "done" }] }))
  .addEdge(START, "work")
  .compile();
const top = new StateGraph(MessagesAnnotation)
  .addNode("team", team)
  .addEdge(START, "team")
  .compile();

const input = { messages: [{ role: "user", content: "go" }] };
for await (const [ns, update] of await top.stream(input, { streamMode: "updates", subgraphs: true })) {
  const [node, values] = Object.entries(update)[0];
  console.log(ns.length ? "inside" : "top", node, Object.keys(values));
}
```

- **✓** inside work ["notes", "messages"], dann top team ["messages"]: zuerst das eigene Update des Teams, dann das, was zum obersten Graphen zurückkommt.
- top team ["notes", "messages"]: Der Stream zeigt nur den obersten Graphen, und die Keys des Subgraphen kommen mit ihm zurück.
- inside work ["notes", "messages"], dann top team ["notes", "messages"]: Jeder Key, den das Team geschrieben hat, kommt zurück.
- Es wirft einen Fehler: Der oberste Graph hat keinen Key notes, also kann das Team keinen schreiben.

**Warum:** Mit subgraphs: true liefert der Stream auch die eigenen Updates des Subgraphen, unter seinem Namespace ["team:<task id>"]; ohne das sähest du nur die Zeile top. Das Team ist selbst der Knoten, also kommen die Keys zurück, die es teilt: messages. notes gibt es nur im Team, also bleibt es dort, und es dort zu schreiben ist erlaubt.

## Was du jetzt kannst

- Einen Subgraphen als Knoten hinzufügen, wenn er Keys teilt, oder ihn aus einem Wrapper-Knoten aufrufen, der den State abbildet.
- Mit subgraphs: true streamen und den Namespace jedes Updates lesen; den State eines pausierten Subgraphen mit getState lesen.
- Mit Command.PARENT die Kontrolle an einen Knoten des Eltern-Graphen abgeben und Supervisoren von Supervisoren bauen.
- Zwischen Subagenten, Übergaben, Skills, Router und eigenem Workflow nach ihren Modellaufrufen und Tokens wählen.

---

[Zurück zu Agenten für Fortgeschrittene](README.md) · [Weiter: Zeitreise](04-time-travel.md)
