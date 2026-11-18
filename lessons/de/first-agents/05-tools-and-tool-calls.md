# Tools und Tool-Calls

Bau deine ersten Agenten · Kapitel 5 von 8 · 20 Minuten

Tools mit zod definieren, an ein Modell binden und die Tool-Calls des Modells verarbeiten.

## Ziele

- Füg dem Schema eines Tools ein beschriebenes Feld hinzu und sieh, wie sich sein JSON Schema ändert.
- Sende ungültige Argumente und sieh, wie die Validierung sie ablehnt.
- Besteh den Check.

## Der Text

### Ein Tool ist eine Funktion mit Schema

tool(fn, { name, description, schema }) aus @langchain/core/tools macht aus einer async-Funktion ein Tool. Das Schema ist ein zod-Objekt: z.object({ order\_id: z.string().describe("The order number") }). Es legt fest, welche Argumente das Tool nimmt, welche Typen sie haben und welche optional sind.

Das Schema dient zweimal. LangChain wandelt es in JSON Schema um, und das liest das Modell. Und dein Code prüft damit die Argumente, die das Modell schickt, bevor deine Funktion läuft.

### Was das Modell sieht

model.bindTools(tools) gibt ein Modell zurück, das bei jedem Aufruf Name, Beschreibung und JSON Schema jedes Tools mitschickt. Die Funktion selbst verlässt deinen Code nie. Das Modell weiß nur, was diese drei ihm sagen, also sind Beschreibungen Prompts: Schreib sie für das Modell und beschreib jedes Feld mit .describe().

### Ein Tool-Call und seine Antwort

Will das Modell ein Tool, ist seine Antwort eine AIMessage mit tool\_calls: Jeder Call hat name, args und eine id. Dein Code führt das Tool aus und antwortet mit einer ToolMessage, die dieselbe tool\_call\_id trägt. Ein Modell kann mehrere Calls auf einmal anfordern, daher ordnet die id jedes Ergebnis seinem Call zu.

Gib den ganzen Tool-Call an tool.invoke(). Das Tool validiert die args gegen sein Schema, führt deine Funktion aus und gibt dir die ToolMessage zurück. Passen die args nicht, wirft es ToolInputParsingException, und deine Funktion läuft nie. Ein Agent-Loop fängt den Fehler ab und schickt ihn als ToolMessage mit status "error" zurück, damit das Modell seinen Call korrigieren kann. In Kapitel 6 erledigt ToolNode das für dich.

## Probier es aus: Von zod zum Tool-Call

Füg dem Schema eines Tools ein Feld hinzu und sieh, was das Modell liest. Führ dann den Call des Modells durch die echte Validierung, einmal mit gültigen Argumenten und einmal mit Argumenten, die du wählst.

https://agenticademy.org/de/paths/first-agents/05-tools-and-tool-calls

## Check

### Frage 1 von 3

Was erhält das Modell über ein gebundenes Tool?

- Den Quellcode des Tools.
- **✓** Seinen Namen, seine Beschreibung und sein JSON Schema.
- Nur seinen Namen.

**Warum:** bindTools wandelt jedes Tool in das Format seines Anbieters um: Name, Beschreibung und Eingabeschema. Die Funktion selbst verlässt deinen Code nie.

### Frage 2 von 3

Was verbindet eine ToolMessage mit dem Call, auf den sie antwortet?

- Der Name des Tools.
- **✓** Die tool\_call\_id, gleich der id des Calls in der AIMessage.
- Ihre Position in der Liste.

**Warum:** Ein Modell kann mehrere Calls auf einmal anfordern, sogar für dasselbe Tool. Die id ordnet jedes Ergebnis seinem Call zu.

### Frage 3 von 3

Das Modell schickt order\_id als Zahl, aber das Schema sagt z.string(). Was passiert?

- Das Tool läuft und wandelt sie um.
- **✓** Die Validierung schlägt fehl, bevor deine Funktion läuft, und der Fehler kann an das Modell zurückgehen.
- Das Modell wird neu trainiert.

**Warum:** Das Schema wird in deinem Code durchgesetzt. Ein Loop kann den Fehler als ToolMessage zurückgeben, damit das Modell es erneut versucht.

## Was du jetzt kannst

- Ein Tool mit tool() und einem zod-Schema definieren, mit hilfreichen Beschreibungen.
- Tools an ein Modell binden und seine tool\_calls lesen.
- Einen Tool-Call mit einer ToolMessage beantworten und ungültige Argumente behandeln.

---

[Zurück zu Bau deine ersten Agenten](README.md) · [Weiter: Der Agent-Loop: ein minimaler ReAct-Agent](06-the-agent-loop.md)
