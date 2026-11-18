# Ein Deep-Agent-Harness

Agenten für Fortgeschrittene · Kapitel 9 von 10 · 20 Minuten

Ein Deep Agent plant mit einer Todo-Liste, hält Notizen in virtuellen Dateien fest, gibt Teile einer Aufgabe an Subagenten mit frischem Kontext ab und hält vor den Tools an, die du nennst.

## Ziele

- Starte den Auftrag und sieh zu, wie der Agent seine Todo-Liste schreibt und aktualisiert.
- Genehmige das Schreiben der Notizen und sieh zu, wie der Dateibaum wächst.
- Lass drei Subagenten parallel laufen und vergleich ihre Kontextgrößen.
- Besteh den Check.

## Der Text

### Ein Harness für lange Aufträge

Ein Harness ist der Code rund um die Modellschleife: der Prompt, die Tools und was jeder Aufruf sieht. LangChains Open-Source-Paket deepagents ist einer, gebaut auf LangGraph. Seine Ankündigung von 2025 nannte vier Teile: einen ausführlichen Prompt, ein Planungstool, Subagenten und ein Dateisystem.

### Todos und Dateien

write\_todos({ todos }) nimmt jedes Mal die ganze Liste, jeden Eintrag pending, in\_progress oder completed, und speichert sie in state.todos. Mehr tut es nicht: Der Gewinn ist, dass das Modell seinen Plan bei jedem Aufruf neu liest. Zwei Aufrufe in einem Zug werden abgelehnt. Eingeschaltet wird es, indem du LangChains todoListMiddleware() zu middleware hinzufügst.

Die Datei-Tools (ls, read\_file, write\_file, edit\_file, glob, grep) arbeiten auf state.files, einem virtuellen Dateisystem im State des Graphen. Subagenten können die Notizen dort lesen, und sie überdauern eine Zusammenfassung alter Nachrichten.

### Subagenten

task({ description, subagent\_type }) startet einen Subagenten mit frischem Kontext: sein eigener System-Prompt und die Beschreibung, keine der Nachrichten des Aufrufers. Er startet mit den Dateien des Aufrufers, die Dateien, die er schreibt, werden zurückgeführt, und nur seine letzte Nachricht kommt zurück, als Tool-Ergebnis. Aufrufe in einem Zug laufen parallel. Ein Allzweck-Subagent mit den Tools des Aufrufers ist standardmäßig dabei.

### Skills, Gedächtnis und Genehmigungen

Ein Skill ist ein Ordner mit einer Datei SKILL.md. Mit skills: ['/skills/'] listet der Prompt Namen und Beschreibung jedes Skills, und der Agent liest die Datei erst, wenn eine Aufgabe sie braucht. Die Option memory lädt AGENTS.md-Dateien in den Prompt. interruptOn nennt, zusammen mit einem Checkpointer, Tools, die den Lauf anhalten, bis eine Person jeden Aufruf genehmigt, bearbeitet oder ablehnt. Diese Seite hält an einem Breakpoint an (interruptBefore), weil interrupt() AsyncLocalStorage braucht.

### Weniger Gerüst

Jeder Teil kostet bei jedem Aufruf Tokens. deepagents 1.12 (2026-07-29) machte die Todo-Liste optional und ließ seinen generischen System-Prompt leer: Aktuelle Modelle brauchen weniger Anleitung. Anthropics Beitrag zum Harness-Design (2026-03-24) sieht es genauso: Entferne mit jedem neuen Modell die Teile, die es nicht mehr braucht.

### In diesem Lab

Ein nachgebauter Harness schreibt ein Briefing zu Erstattungsbeschwerden bei Halden, einem erfundenen Shop. Anders als deepagents 1.16 schaltet er die Liste mit todos: true ein, zeigt dem Researcher die Skills ohne eigene Skill-Liste, behält die alte generische Prompt-Zeile und hat kein AGENTS.md-Gedächtnis. Der Lead hält vor write\_file und task an, damit du ihm folgen kannst; in deinem eigenen Agenten halte vor Tools an, die etwas außerhalb von ihm ändern.

## Probier es aus: Ein Rechercheauftrag, ein Teil nach dem anderen

Starte den Auftrag und beobachte die Todo-Liste, lass den Lead seine Notizen schreiben, genehmige dann drei Subagenten und vergleich ihre Kontextgrößen.

https://agenticademy.org/de/paths/advanced-agents/09-deep-agent-harness

## Check

Der Kontext des Leads umfasst etwa 1.400 Tokens, wenn sein Modell diesen Aufruf macht. Der Researcher liest etwa 600 Tokens aus Dateien, bearbeitet die Notiz und endet mit einem Bericht von 40 Tokens. Was ändert sich im State des Leads?

Code:

```
const lead = createDeepAgent({
  systemPrompt: 'You write briefs.',
  subagents: [{
    name: 'researcher',
    description: 'Researches a cause.',
    systemPrompt: 'Edit the note, then report.',
  }],
});

// The lead's model asks for:
task({
  subagent_type: 'researcher',
  description:
    'Late refunds: /notes/late-refund.md',
})
```

- Seine Nachrichten bekommen das ganze Gespräch des Researchers, etwa 650 Tokens; seine Dateien bleiben gleich.
- Seine Nachrichten bekommen ein Tool-Ergebnis, den Bericht von 40 Tokens; seine Dateien bleiben gleich, weil der Researcher eine Kopie bearbeitet hat.
- **✓** Seine Nachrichten bekommen ein Tool-Ergebnis, den Bericht von 40 Tokens, und die bearbeitete Notiz wird in seine Dateien zurückgeführt.
- Nichts ändert sich, bis der Lead /notes/late-refund.md selbst liest.

**Warum:** task startet den Researcher mit frischem Kontext: sein eigener System-Prompt und die Beschreibung, keine der Nachrichten des Leads. Was er liest, bleibt in seinem eigenen Kontext. Nur seine letzte Nachricht kommt zurück, als Ergebnis des task-Tools, also wächst der Lead um etwa 40 Tokens, und die Dateien, die der Researcher geschrieben hat, werden in state.files des Leads zurückgeführt. Im Lab lasen drei Researcher Tausende Tokens, während der Lead um drei kurze Berichte wuchs.

## Was du jetzt kannst

- Die Todo-Liste eines Deep Agents einschalten, indem du todoListMiddleware() zu seiner middleware hinzufügst, und den Plan aus state.todos lesen.
- Notizen und Entwürfe mit ls, read\_file, write\_file, edit\_file, glob und grep in state.files ablegen.
- In sich geschlossene Arbeit an task-Subagenten abgeben und die Berichte und Dateien nutzen, die zurückkommen.
- Einen Deep Agent mit interruptOn und einem Checkpointer vor ausgewählten Tools anhalten und Gerüst entfernen, das ein neueres Modell nicht mehr braucht.

---

[Zurück zu Agenten für Fortgeschrittene](README.md) · [Weiter: Agenten mit langem Horizont](10-long-horizon.md)
