# Ein Modellaufruf

Bau deine ersten Agenten · Kapitel 2 von 8 · 15 Minuten

Ein Chat-Modell mit LangChain.js aufrufen, die zurückgegebene AIMessage lesen und den eigenen Key sicher nutzen.

## Ziele

- Führ den Aufruf mit dem geskripteten Modell aus.
- Finde die Token-Zahlen in der Antwort.
- Sag voraus, was der Code ausgibt.

## Der Text

### Eine Schnittstelle, viele Anbieter

Jedes Anbieterpaket hat eine Chat-Modell-Klasse, etwa ChatAnthropic oder ChatOpenAI. Sie teilen eine Schnittstelle: invoke(messages) schickt eine Liste von Nachrichten und gibt eine AIMessage zurück. Du kannst auch einen einfachen String übergeben, den LangChain in eine HumanMessage verpackt. stream(messages) liefert dieselbe Antwort in Stücken, sobald sie ankommen, sodass du Text zeigen kannst, während das Modell noch schreibt.

Optionen gehen an den Konstruktor: model (der Name des Modells bei diesem Anbieter), temperature (0 ergibt die vorhersehbarste Formulierung; höhere Werte variieren mehr) und maxTokens (die längste Antwort, die du bezahlst). ChatOpenAI nimmt auch configuration.baseURL, sodass dieselbe Klasse mit jedem OpenAI-kompatiblen Dienst spricht: OpenRouter, dem Router von Hugging Face oder dem API-Katalog von NVIDIA.

### Was zurückkommt

Die AIMessage hat content (den Text, bei einer einfachen Antwort), tool\_calls (leer bis Kapitel 5), usage\_metadata und response\_metadata. usage\_metadata zählt Tokens, die Textstücke, die ein Modell liest und schreibt: input\_tokens für das, was du gesendet hast, output\_tokens für die Antwort. Anbieter rechnen nach Tokens ab, und die Eingabe wächst mit jeder Nachricht, die du sendest, also ist das die Zahl, die du im Blick behalten solltest. response\_metadata enthält, was der Anbieter ergänzt, etwa den Modellnamen und warum es aufgehört hat.

### Keys

Ein API-Key ist ein Passwort für ein kostenpflichtiges Konto. Halte ihn aus dem Code und aus Repositories heraus: Lies ihn auf deinem Rechner oder Server aus einer Umgebungsvariable. Auf dieser Seite bleibt ein Key, den du verbindest, in diesem Tab und geht von deinem Browser direkt zum Anbieter. Zum Lernen ist das in Ordnung, aber eine öffentliche App sollte Modelle von einem Server aus aufrufen, damit ihr Key nie an Besucher gesendet wird. Es gibt kostenlose Stufen, und sie haben Rate-Limits.

## Probier es aus: Eine Anfrage und ihre Antwort

Führ den Aufruf aus und öffne die AIMessage, die zurückkommt. Verbinde dein eigenes Modell, wenn du eine Live-Antwort sehen willst.

https://agenticademy.org/de/paths/first-agents/02-one-model-call

## Check

Das Modell ist ein Chat-Modell, das mit "Hello." antwortet und seinen Verbrauch meldet. Was gibt dieser Code aus?

Code:

```
const reply = await model.invoke("Say hello.");
console.log(typeof reply.content, reply.usage_metadata.output_tokens > 0);
```

- **✓** string true
- object true
- string undefined
- Es wirft einen Fehler: invoke braucht ein Array von Nachrichten.

**Warum:** invoke nimmt auch einen einfachen String an, den LangChain in eine HumanMessage verpackt. Bei einer Textantwort ist content ein String, und usage\_metadata zählt die Output-Tokens.

## Was du jetzt kannst

- Ein Chat-Modell mit invoke() und stream() aufrufen.
- Inhalt und Token-Verbrauch aus einer AIMessage lesen.
- ChatOpenAI mit baseURL auf einen OpenAI-kompatiblen Dienst richten.
- Erklären, wo ein API-Key liegen sollte und wo nicht.

---

[Zurück zu Bau deine ersten Agenten](README.md) · [Weiter: Nachrichten und State](03-messages-and-state.md)
