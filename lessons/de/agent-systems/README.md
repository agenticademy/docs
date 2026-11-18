# Agentensysteme und Standards

Mittelstufe · Für Entwickler · 8 Kapitel · 195 Min.

Für Entwickler, die schon einen einzelnen Agenten bauen können. Du verbindest Agenten mit offenen Standards mit Tools, mit anderen Agenten, mit Oberflächen und mit Zahlungen, orchestrierst mehrere Agenten in einem Graph, entscheidest, was in jeden Kontext kommt, tracest Läufe und testest sie gegen Angriffe. Die Protokollbibliotheken laufen echt in deinem Browser; du kannst jede Nachricht anhalten, verwerfen und manipulieren.

## Bevor du anfängst

Der Lernpfad „Bau deine ersten Agenten“ oder dasselbe Wissen: Nachrichten und Tools in LangChain.js, Graphen in LangGraph und die Agentenschleife.

## Am Ende kannst du

- Einen Agenten mit dem Protokoll 2026-07-28 an einen MCP-Server anbinden, einschließlich Anfragen nach der Eingabe eines Menschen.
- Mit A2A-Tasks und Agent Cards Arbeit an einen anderen Agenten delegieren.
- Die Arbeit eines Agenten mit AG-UI an eine Oberfläche streamen und sichere UI mit A2UI beschreiben.
- Erklären, wie UCP und AP2 den Kauf eines Agenten an die signierte Zustimmung eines Menschen binden.
- Agenten mit Supervisoren, Send-Fan-out und Subgraphen orchestrieren.
- Kontext mit Suche, Auslagerung und Subagenten gestalten und Läufe im Format von LangSmith tracen.
- Ein Agentensystem gegen Prompt-Injection und Manipulation testen, mit den agentischen OWASP-Risiken als Karte.
- Eine Superstep-Zeitleiste und einen Run-Baum lesen.
- Einem Agenten ein Aufruflimit, ein Tool-Limit oder einen Schutz als Middleware hinzufügen.

## Kapitel

1. [MCP: Tools von einem Server](01-mcp.md): Wie ein Agent Tools auf einem MCP-Server findet und aufruft, und wie das Protokoll 2026-07-28 vor einem riskanten Aufruf einen Menschen fragt.
2. [A2A: Agenten, die an Agenten delegieren](02-a2a.md): Wie ein Agent einen anderen über dessen Agent Card findet und ihm einen Task übergibt, den er bis zum Ende verfolgen kann.
3. [Agenten und Oberflächen: AG-UI und A2UI](03-agents-and-uis.md): Die Arbeit eines Agenten mit AG-UI an eine App streamen und einen Agenten mit A2UI sichere Oberflächenkomponenten beschreiben lassen.
4. [Agenten, die bezahlen: UCP und AP2](04-agent-commerce.md): Wie ein Agent bei einem Händler über UCP zur Kasse geht und wie AP2-Mandate die Zahlung an das binden, was der Mensch signiert hat.
5. [Orchestrierung: Supervisor, Fan-out, Subgraphen](05-orchestration.md): Muster für mehrere Agenten in einem LangGraph-Graphen: ein Supervisor, der weiterleitet, Send für parallele Arbeit, Subgraphen und der Checkpoint, den jeder Superstep speichert.
6. [Context Engineering und Retrieval](06-context-engineering.md): Auswählen, was jeder Modellaufruf liest: grep oder Vektorsuche, große Ergebnisse auslagern, ein erster Blick auf Subagenten und ein Gedächtnis, das über Threads hinweg hält.
7. [Observability: Traces und Läufe](07-observability.md): Jeden Modellaufruf, Tool-Call und Schritt als Baum von Läufen aufzeichnen und ihn lesen, um zu finden, was langsam oder kaputt ist.
8. [Evaluation und Sicherheit](08-evaluation-and-safety.md): Ein Agentensystem mit Fällen und Checks testen, es mit Injection und Manipulation angreifen und die Fehler den agentischen Risiken von OWASP zuordnen.

https://agenticademy.org/de/paths/agent-systems
