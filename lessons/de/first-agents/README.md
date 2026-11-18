# Bau deine ersten Agenten

Einstieg · Für Entwickler · 8 Kapitel · 152 Min.

Für Entwickler, die mit Agenten anfangen. Du baust Schritt für Schritt einen kleinen Support-Agenten: ein Modellaufruf, Nachrichten und State, ein Graph, Tools, die Agenten-Schleife, Callbacks mit menschlicher Freigabe und Guardrails. Jedes Kapitel führt die echten Bibliotheken in deinem Browser mit einem geskripteten Modell aus, du brauchst also keinen Key; du kannst dein eigenes Modell verbinden, wenn du willst.

## Bevor du anfängst

Etwas JavaScript: Funktionen, Objekte, async/await und das Installieren von Paketen mit npm.

## Am Ende kannst du

- Die Pakete von LangChain.js und LangGraph.js für einen Agenten wählen und wissen, was jedes tut.
- Ein Chat-Modell aufrufen und mit Nachrichten und Graph-State arbeiten.
- Einen StateGraph mit Knoten, Kanten und bedingtem Routing bauen.
- Tools mit zod-Schemas definieren und einen minimalen ReAct-Agenten ausführen.
- Einen Lauf mit Checkpoints und interruptBefore für eine menschliche Freigabe pausieren und ihn mit Callbacks protokollieren.
- Guardrails hinzufügen: Eingabe- und Ausgabeprüfungen, Allow-Lists für Tools und Schrittlimits.

## Kapitel

1. [Die Bibliotheken, und was hier läuft](01-libraries-and-setup.md): Welche Pakete von LangChain.js und LangGraph.js ein Agent braucht, was jedes davon tut und wie sie im Browser laufen.
2. [Ein Modellaufruf](02-one-model-call.md): Ein Chat-Modell mit LangChain.js aufrufen, die zurückgegebene AIMessage lesen und den eigenen Key sicher nutzen.
3. [Nachrichten und State](03-messages-and-state.md): Die vier Nachrichtentypen, warum jedes Mal die ganze Unterhaltung gesendet wird und wie LangGraph State mit Reducern zusammenführt.
4. [Dein erster Graph](04-your-first-graph.md): Einen StateGraph aus Knoten und Kanten bauen, mit einer bedingten Kante routen und ihn Schritt für Schritt ausführen.
5. [Tools und Tool-Calls](05-tools-and-tool-calls.md): Tools mit zod definieren, an ein Modell binden und die Tool-Calls des Modells verarbeiten.
6. [Der Agent-Loop: ein minimaler ReAct-Agent](06-the-agent-loop.md): Ein Modell-Knoten, ein Tool-Knoten und eine bedingte Kante: der ganze Agent-Loop als LangGraph-Graph.
7. [Callbacks und menschliche Freigabe](07-callbacks-and-approval.md): Einen Lauf mit Callbacks beobachten und ihn vor einem riskanten Tool anhalten, damit ein Mensch ihn freigeben oder ablehnen kann.
8. [Guardrails](08-guardrails.md): Prüfungen im Code rund um das Modell: Eingabe- und Ausgabeprüfungen, Tool-Allow-Lists und Schrittlimits, in Schichten.

https://agenticademy.org/de/paths/first-agents
