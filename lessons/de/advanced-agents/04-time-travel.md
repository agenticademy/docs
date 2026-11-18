# Zeitreise

Agenten für Fortgeschrittene · Kapitel 4 von 10 · 20 Minuten

Jeder Superstep speichert einen Checkpoint, also kannst du die Geschichte eines Laufs lesen, ab einem früheren Schritt neu abspielen, einen früheren State bearbeiten und ein zweites Ende abzweigen.

## Ziele

- Führe einen Graphen mit fünf Knoten auf einem Thread aus und lies seine Checkpoint-Geschichte.
- Spule zu Schritt 3 zurück und spiele ab dort neu ab.
- Bearbeite den State in Schritt 3, zweige ab und vergleiche beide Enden.
- Bring die Schritte einer Abzweigung in die richtige Reihenfolge.

## Der Text

### Ein Thread ist eine Zeitleiste

Kompiliere einen Graphen mit einem Checkpointer, führe ihn mit einer thread\_id aus, und LangGraph speichert nach jedem Superstep einen Checkpoint des ganzen States, wie im Lernpfad Agentensysteme. Jeder Checkpoint hat eine eigene checkpoint\_id, in seinen Metadaten einen step und eine source (step -1 ist der Input) und eine parentConfig, die den Checkpoint davor nennt. Über ihre Parents verknüpft, bilden die Checkpoints eines Threads eine Zeitleiste, und eine Zeitleiste kann sich verzweigen.

### Die Vergangenheit lesen

getStateHistory({ configurable: { thread\_id } }) listet alle Checkpoints des Threads auf, die neuesten zuerst. Jeder Eintrag ist ein Snapshot des States: values, next (die Knoten, die als Nächstes laufen), config und metadata. getState mit einer checkpoint\_id in seiner config liest einen davon. Das ist Zurückspulen: Du siehst dir einen früheren State an und änderst nichts.

### Neu abspielen und abzweigen

invoke(null, config) mit einer config, die einen früheren Checkpoint nennt, spielt ab dort neu ab. Knoten vor diesem Checkpoint laufen nicht noch einmal; die danach schon, also kann ein echtes Modell beim zweiten Mal anders antworten. Die neuen Checkpoints wachsen auf einem neuen Zweig, der mit einer Kopie mit der source fork beginnt. Die alten bleiben.

updateState(config, values, asNode) ändert die Vergangenheit, ohne sie zu löschen. Es schreibt einen neuen Checkpoint mit der source update auf den, den config nennt, als hätte der Knoten asNode values zurückgegeben, und gibt die config des neuen Checkpoints zurück. Führe invoke(null, thatConfig) aus, und der Thread verzweigt sich: Das erste Ende bleibt in der Geschichte, und ein zweites wächst aus deiner Änderung. getState ohne checkpoint\_id liest den neuesten Checkpoint, der jetzt das Ende der Abzweigung ist.

### Durability

Die Option durability eines Laufs bestimmt, wann Checkpoints geschrieben werden. "async", der Standard, speichert jeden, während der nächste Superstep läuft; "sync" speichert ihn, bevor der nächste Superstep beginnt; "exit" speichert nur, wenn der Lauf endet. "exit" schreibt am wenigsten, lässt aber keinen früheren Schritt zum Zurückspulen übrig und nichts zum Fortsetzen, wenn der Prozess mittendrin abstürzt.

### In diesem Lab

Haldens Erstattungsprüfung für das Ticket T-48213 führt fünf Knoten nacheinander aus: read\_ticket, classify, list\_charges, decide und draft\_reply. LangGraph.js 1.5.1 und sein MemorySaver laufen auf dieser Seite, und classify, decide und draft\_reply fragen ein geskriptetes Modell. Der Browser-Build von LangGraph.js hat kein AsyncLocalStorage, also gibt jeder Knoten seine config ausdrücklich an das Modell weiter.

## Probier es aus: Eine Checkpoint-Schiene

Drei Schritte: Führe den Graphen aus und lies seine Checkpoints, spule zu Schritt 3 zurück und spiele neu ab, dann bearbeite Schritt 3, zweige ab und vergleiche beide Enden.

https://agenticademy.org/de/paths/advanced-agents/04-time-travel

## Check

Bring die Schritte, um einen Lauf ab einem früheren Checkpoint abzuzweigen, in die richtige Reihenfolge.

1. Führe den Graphen mit einem Checkpointer und einer thread\_id aus.
2. Liste die Checkpoints des Threads mit getStateHistory auf.
3. Wähle den Checkpoint, zu dem du zurückwillst, über seine checkpoint\_id.
4. Rufe updateState mit der config dieses Checkpoints und den neuen Werten auf.
5. Rufe invoke(null, config) mit der config auf, die updateState zurückgegeben hat.
6. Vergleiche das Ende der Abzweigung mit dem ersten, das noch in der Geschichte steht.

**Warum:** Checkpoints gibt es erst, wenn ein Lauf auf einem Thread sie gespeichert hat, und einen früheren findest du in getStateHistory über seine checkpoint\_id. updateState braucht die config dieses Checkpoints und gibt die config des neuen Checkpoints zurück, den es schreibt; invoke(null, …) mit dieser config führt die Abzweigung aus. Nichts wird überschrieben, also ist das erste Ende noch zum Vergleichen da.

## Was du jetzt kannst

- Die Checkpoints eines Threads mit getStateHistory lesen, und einen davon mit getState und seiner checkpoint\_id.
- Einen Lauf ab einem früheren Checkpoint neu abspielen und sagen, welche Knoten erneut laufen.
- Einen früheren State mit updateState bearbeiten, den Thread abzweigen und beide Enden vergleichen.
- Einen Durability-Modus wählen und wissen, wohin jeder dich zurückspulen lässt.

---

[Zurück zu Agenten für Fortgeschrittene](README.md) · [Weiter: Schleifen, die durchhalten](05-loops-that-survive.md)
