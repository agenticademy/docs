# MCP: Tools von einem Server

Agentensysteme und Standards · Kapitel 1 von 8 · 25 Minuten

Wie ein Agent Tools auf einem MCP-Server findet und aufruft, und wie das Protokoll 2026-07-28 vor einem riskanten Aufruf einen Menschen fragt.

## Ziele

- Führ das Szenario aus, bis die Erstattung erfolgt ist.
- Beantworte die Bestätigung des Helpdesks selbst.
- Manipuliere eine Nachricht und sieh, wie das echte SDK sie ablehnt.
- Besteh den Check.

## Der Text

### Hosts, Clients und Server

Das Model Context Protocol (MCP) ist ein offener Standard, der KI-Anwendungen mit Tools und Daten verbindet. Die Anwendung, Host genannt, betreibt einen MCP-Client pro Server, den sie nutzt. Ein Server bietet Tools (Aktionen, die das Modell aufrufen kann), Ressourcen (Daten, die die Anwendung lesen kann) und Prompts (Vorlagen, die ein Mensch wählen kann). Nachrichten sind JSON-RPC 2.0, über HTTP oder über die Standardein- und -ausgabe eines lokalen Prozesses (stdio).

Im Agenten aus dem vorigen Lernpfad waren Tools Funktionen in deinem Code. Mit MCP liegen sie in einem Server, den jemand anderes schreiben und betreiben kann, und jeder MCP-Host kann sie nutzen.

### Was sich mit 2026-07-28 geändert hat

Dieses Kapitel nutzt die Protokollversion 2026-07-28 mit dem offiziellen TypeScript SDK v2. Du siehst drei Änderungen auf der Leitung.

Discovery: Der Client ruft server/discover auf, um die Versionen und Fähigkeiten des Servers zu erfahren. Server mit den Versionen von 2025 antworten stattdessen auf initialize, und das SDK greift darauf zurück.

Eine Version in jeder Anfrage: Jede Anfrage trägt ihre Protokollversion in \_meta, also hält der Server keine Session, und jede seiner Instanzen kann jede Anfrage beantworten.

Eingabe erforderlich: Braucht ein Tool die Antwort eines Menschen, beendet der Server die Anfrage mit input\_required, der Frage und einem signierten requestState. Der Client fragt den Menschen und ruft das Tool dann erneut mit der Antwort und demselben requestState auf. Der Server prüft die Signatur, also muss der erneute Aufruf dem entsprechen, was der Mensch gesehen hat.

### Tools sind ein Vertrag

Das Modell sieht nur den Namen, die Beschreibung und das JSON Schema eines Tools. Annotationen wie readOnlyHint und destructiveHint sind Hinweise des Servers: Ein Client vertraut ihnen nur so weit, wie er diesem Server vertraut. Die Erstattung in diesem Lab ist sicher, weil der Server selbst eine Bestätigung verlangt und prüft, nicht weil sich das Modell gut verhält.

### Was du hier machst

Haldens Support-Agent, ein LangChain-Agent mit dem Client des SDK, und Haldens Helpdesk, der Server des SDK, laufen beide auf dieser Seite. Führ das Szenario aus und beantworte die Erstattungsfrage selbst. Schalte dann Abfangen ein, halte eine Nachricht an und manipuliere sie: Der echte SDK-Code lehnt sie ab.

## Probier es aus: Die Leitung: ein Agent und ein MCP-Helpdesk

Vier Schritte mit je einer Aktion, dazu ein optionaler fünfter. Das Diagramm unter den Schritten hat eine Spalte pro Beteiligtem und eine Zeile pro Nachricht, in der Reihenfolge, in der sie gesendet wird, von oben nach unten.

https://agenticademy.org/de/paths/agent-systems/01-mcp

## Check

### Frage 1 von 4

Wer betreibt bei MCP den Client?

- Der Server.
- **✓** Die Host-Anwendung, zum Beispiel der Support-Agent.
- Der Modellanbieter.

**Warum:** Der Host, etwa ein Agent oder eine IDE, betreibt einen Client pro Server, mit dem er sich verbindet. Das Modell spricht nie direkt mit dem Server; das tut der Host, wenn das Modell ein Tool anfordert.

### Frage 2 von 4

Der Helpdesk braucht vor einer Erstattung das OK des Support-Leads. Was sendet ein Server nach 2026-07-28?

- Eine neue Anfrage vom Server an den Client, die nach einer Eingabe fragt.
- **✓** Ein Ergebnis mit input\_required, der Frage und einem signierten requestState, das die Anfrage beendet.
- Fehler -32602.

**Warum:** Der Server beendet die Anfrage, statt auf einer offenen Verbindung zu warten. Der Client fragt den Menschen und ruft erneut mit der Antwort und demselben requestState auf, also kann jede Server-Instanz die Arbeit abschließen.

### Frage 3 von 4

Beim erneuten Aufruf erhöhst du den Erstattungsbetrag. Warum lehnt der Server ab?

- Das Modell bemerkt die Änderung.
- **✓** Die Argumente passen nicht mehr zum signierten requestState, der für das ausgestellt wurde, was der Mensch freigegeben hat.
- MCP erlaubt keine Beträge in Argumenten.

**Warum:** requestState ist vom Server signiert (hier mit HMAC). Der Server liest die freigegebene Bestellung und den Betrag zurück und verweigert jeden Aufruf, der davon abweicht.

### Frage 4 von 4

Ein Server markiert ein Tool mit readOnlyHint: true. Was sollte der Client tun?

- Es ausführen, ohne je jemanden zu fragen, da es nichts ändern kann.
- **✓** Es als Hinweis behandeln, dem er nur so weit vertraut, wie er diesem Server vertraut.
- Die Ausführung verweigern.

**Warum:** Annotationen beschreiben, was der Server behauptet. Ein bösartiger oder fehlerhafter Server kann ein Tool falsch kennzeichnen, also entscheidet die eigene Richtlinie des Clients, was eine Freigabe braucht.

## Was du jetzt kannst

- Einen MCP-Host, -Client und -Server beschreiben und die drei Dinge, die ein Server anbietet.
- Dem Ablauf nach 2026-07-28 folgen: server/discover, tools/list, tools/call, input\_required und der erneute Aufruf.
- Erklären, warum ein signierter requestState eine Bestätigung an das bindet, was der Mensch gesehen hat.
- Tool-Annotationen als Hinweise behandeln, nicht als Garantien.

---

[Zurück zu Agentensysteme und Standards](README.md) · [Weiter: A2A: Agenten, die an Agenten delegieren](02-a2a.md)
