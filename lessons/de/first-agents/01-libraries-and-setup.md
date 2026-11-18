# Die Bibliotheken, und was hier läuft

Bau deine ersten Agenten · Kapitel 1 von 8 · 15 Minuten

Welche Pakete von LangChain.js und LangGraph.js ein Agent braucht, was jedes davon tut und wie sie im Browser laufen.

## Ziele

- Öffne die sechs Pakete in der Karte und lies, was dir jedes Paket gibt.
- Führ das Hello-World auf dieser Seite aus und lies die Antwort.
- Besteh den Check.

## Der Text

### Die Pakete

LangChain.js und LangGraph.js sind Open-Source-Bibliotheken für JavaScript, mit denen du Anwendungen mit Sprachmodellen baust. Sie kommen als getrennte Pakete, sodass du nur installierst, was du nutzt.

@langchain/core enthält die gemeinsamen Bausteine: Nachrichten, die Schnittstelle für Chat-Modelle, Tools und Callbacks. @langchain/langgraph ist die Runtime für Agenten als Graphen: State, Knoten, Kanten, Checkpoints und Pausen für eine Freigabe. Ein Anbieterpaket wie @langchain/anthropic oder @langchain/openai liefert die Chat-Modell-Klasse für die API eines Anbieters; dieselbe Klasse ChatOpenAI spricht auch mit OpenAI-kompatiblen Diensten wie OpenRouter, Hugging Face und NVIDIA. zod beschreibt und validiert die Eingaben deiner Tools; LangChain macht aus einem zod-Schema das JSON Schema, das das Modell liest. langsmith ist optional und zeichnet Traces jedes Laufs auf.

### Wozu eine Graph-Bibliothek

Du kannst eine Agenten-Schleife von Hand schreiben: das Modell aufrufen, die angeforderten Tools ausführen, wiederholen. Kapitel 6 baut diese Schleife als Graphen aus zwei Knoten. LangGraph ergänzt, was einer handgeschriebenen Schleife fehlt: State, der nach jedem Schritt gespeichert wird, sodass ein Lauf für einen Menschen pausieren und später weiterlaufen kann; Zweige, die parallel laufen; und eine Struktur, die du untersuchen und zeichnen kannst.

Das Paket langchain baut auf LangGraph auf und bietet createAgent, einen fertigen Agenten mit Middleware. Dieser Lernpfad baut den Graphen zuerst von Hand, damit du später weißt, was solche Helfer für dich erledigen.

### Installieren

In deinem eigenen Projekt: npm install @langchain/core @langchain/langgraph @langchain/anthropic zod. Dieser Lernpfad nutzt @langchain/core 1.2.18, @langchain/langgraph 1.5.1 und zod 4.6.7. Leg in echten Projekten exakte Versionen fest: Diese Bibliotheken ändern sich schnell.

### Was im Browser anders ist

Alles in diesem Lernpfad läuft in deinem Browser, deshalb wird LangGraph aus @langchain/langgraph/web importiert. Der Haupteinstiegspunkt reicht die Konfiguration mit AsyncLocalStorage an verschachtelte Aufrufe weiter, und das haben Browser nicht. Mit /web übergibst du config explizit an verschachtelte Aufrufe und pausierst Läufe mit Breakpoints (interruptBefore) statt mit interrupt(). Der Rest der API ist gleich.

Das Modell ist standardmäßig geskriptet, du brauchst also keinen Key, und jeder Lauf liefert dasselbe Ergebnis. In diesem Kapitel ist es LangChains eigenes FakeListChatModel, das die Antworten zurückgibt, die du ihm vorgibst. Ab Kapitel 2 kannst du ein echtes Modell mit deinem eigenen Key verbinden.

## Probier es aus: Die Pakete

Öffne jedes Paket in der Karte und sieh, was es dir gibt, welche Version hier läuft und wie du es importierst. Dann führ das Hello-World aus.

https://agenticademy.org/de/paths/first-agents/01-libraries-and-setup

## Check

### Frage 1 von 4

Welches Paket definiert HumanMessage, AIMessage und tool()?

- @langchain/langgraph
- **✓** @langchain/core
- @langchain/anthropic
- zod

**Warum:** @langchain/core enthält die gemeinsamen Bausteine. LangGraph und die Anbieterpakete importieren sie von dort.

### Frage 2 von 4

Du führst einen LangGraph-Graphen im Browser aus. Woher importierst du StateGraph?

- @langchain/langgraph
- langchain
- **✓** @langchain/langgraph/web

**Warum:** Der Einstiegspunkt /web hängt nicht von AsyncLocalStorage ab, das Browsern fehlt. Du übergibst config dann explizit an verschachtelte Aufrufe.

### Frage 3 von 4

Was macht zod in einem LangChain-Agenten?

- **✓** Es beschreibt und validiert Tool-Eingaben; LangChain schickt das Schema als JSON Schema an das Modell.
- Es ruft das Modell auf.
- Es speichert Checkpoints zwischen Schritten.

**Warum:** Das Schema dient doppelt: Das Modell liest es, um zu wissen, welche Argumente es senden soll, und dein Code lehnt damit falsche Argumente ab.

### Frage 4 von 4

Warum nutzt das Hello-World FakeListChatModel?

- Es denkt besser als echte Modelle.
- Echte Modelle lassen sich nicht aus dem Browser aufrufen.
- **✓** Es gibt feste Antworten zurück, also läuft der Code ohne Key und ohne Netzwerk, jedes Mal gleich.

**Warum:** Ein Fake-Modell macht Code testbar und kostenlos ausführbar. Echte Modelle lassen sich mit einem Key aus dem Browser aufrufen, wie Kapitel 2 zeigt.

## Was du jetzt kannst

- Die Pakete für einen LangGraph.js-Agenten wählen und sagen, was jedes tut.
- Sie mit festgelegten Versionen installieren.
- Erklären, was sich ändert, wenn LangGraph im Browser läuft.
- LangChain-Code mit einem Fake-Modell ausführen, ohne Key.

---

[Zurück zu Bau deine ersten Agenten](README.md) · [Weiter: Ein Modellaufruf](02-one-model-call.md)
