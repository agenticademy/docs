# Agenten, die bezahlen: UCP und AP2

Agentensysteme und Standards · Kapitel 4 von 8 · 25 Minuten

Wie ein Agent bei einem Händler über UCP zur Kasse geht und wie AP2-Mandate die Zahlung an das binden, was der Mensch signiert hat.

## Ziele

- Erstelle eine Checkout-Session und schließ sie ab.
- Signiere die Mandate auf dem Handy.
- Ändere den Betrag nach dem Signieren und sieh, wie die Prüfung scheitert.
- Besteh den Check.

## Der Text

### UCP: der Checkout

Mit UCP (Universal Commerce Protocol, Version 2026-08-25) kauft ein Shopping-Agent, die Plattform, bei einem Händler, dem Business. Das Business veröffentlicht ein Profil unter /.well-known/ucp: seine Version, seine Fähigkeiten wie checkout, seine Payment-Handler und seinen REST-Endpoint. Der Agent liest es, erstellt dann mit POST /checkout-sessions eine Checkout-Session, aktualisiert sie mit PUT, bis der Status ready\_for\_complete ist, und schließt mit POST …/complete ab.

Jeder Aufruf trägt einen UCP-Agent-Header, der auf das eigene Profil der Plattform zeigt, damit das Business prüfen kann, was die Plattform unterstützt. Jeder Schreibvorgang trägt einen Idempotency-Key: Schickt der Agent nach einem Timeout dieselbe Anfrage erneut, antwortet das Business mit dem ersten Ergebnis, statt zweimal zu handeln. Eine doppelte Abbuchung wie bei Haldens Bestellung 48213 ist genau das, was er verhindert.

### AP2: die Signatur des Menschen

UCP allein vertraut jedem, der die Session hält. AP2 (Agent Payments Protocol) fügt einen Nachweis der Zustimmung hinzu. Der Anbieter des Agenten stellt zwei Mandate als SD-JWT Verifiable Credentials aus: ein Checkout-Mandat mit einem Hash genau des Checkouts, den Halden signiert hat, und ein Payment-Mandat mit dem Betrag, dem Empfänger und der Karte. Der Mensch prüft sie auf einem Bildschirm, auf den der Agent nicht zeichnen kann, der Wallet seines Handys, und der Schlüssel des Handys signiert auf jedem ein Key-Binding-JWT.

Halden prüft das Checkout-Mandat gegen den Checkout, den es signiert hat. Das Karten-Wallet prüft das Payment-Mandat, bevor es einen Payment-Token für diesen Betrag ausstellt, und der Processor prüft den Token, bevor er abbucht. Ändere den Warenkorb oder den Betrag, nachdem der Mensch signiert hat, und ein Hash oder eine Signatur passt nicht mehr, egal was der Agent sagt.

Hier ist der Mensch dabei. Im autonomen Modus von AP2 ist er es nicht: Er gibt vorab offene Mandate frei, mit Grenzen wie einem Höchstpreis, und der Agent signiert die endgültigen Mandate innerhalb dieser Grenzen.

### ANP, in zwei Sätzen

ANP (Agent Network Protocol) ist für Agenten, die sich im offenen Web treffen: Jeder Agent hat einen did:wba-Identifier, dessen DID-Dokument, von seiner eigenen Domain ausgeliefert, seine öffentlichen Schlüssel listet. Jede Anfrage ist mit HTTP Message Signatures signiert, also weiß die Gegenseite ohne Konto, welcher Agent sie gesendet hat. Die ANP-Labs im Schritt Weiterführend führen es aus.

### Was du hier machst

Ein Shopping-Agent kauft Haldens Zelt Ridge 2. Verfolge den Checkout, gib auf dem Handy frei, führ ihn dann mit eingeschaltetem Abfangen erneut aus und erhöh den Betrag nach dem Signieren.

## Probier es aus: Eine Kette von Signaturen

Fünf Schritte, dann zwei optionale. Unter den Schritten fügt die Kette jedes Glied des Kaufs hinzu, sobald der Kauf es erreicht, mit dem, der es prüft. Unter der Kette zeichnet das Lab jede Nachricht als Zeile, von oben nach unten.

https://agenticademy.org/de/paths/agent-systems/04-agent-commerce

## Check

### Frage 1 von 4

Was bindet ein AP2-Checkout-Mandat?

- Den Namen des Agenten.
- **✓** Einen Hash genau des Checkouts, mit Warenkorb und Summe, den der Mensch freigegeben hat.
- Das Logo des Händlers.

**Warum:** Das Mandat ist ein signierter Nachweis dessen, was der Mensch kaufen wollte. Ein abweichender Checkout hat einen anderen Hash, also passt er nicht mehr.

### Frage 2 von 4

Der Agent erhöht den Betrag, nachdem der Mensch signiert hat. Wo scheitert das?

- Nirgends, wenn der Agent erklärt, warum.
- **✓** Bei der Prüfung: Das Karten-Wallet, Halden oder der Processor stellt fest, dass eine Signatur oder ein Hash nicht mehr passt.
- Erst bei der Bank, Tage später.

**Warum:** Jede Partei prüft die Signaturen gegen die Werte, die vor ihr liegen. Der Agent kann sich nicht an einem Hash vorbeireden.

### Frage 3 von 4

Was verhindert der Idempotency-Key von UCP?

- Prompt-Injection.
- **✓** Dass eine wiederholte Anfrage eine zweite Bestellung oder Abbuchung erzeugt.
- Abgelaufene Karten.

**Warum:** Ein Netzwerk-Retry wiederholt eine Anfrage. Mit demselben Key gibt das Business das erste Ergebnis zurück, statt zweimal zu handeln: Die doppelte Abbuchung in Haldens Geschichte ist genau das, was er verhindert.

### Frage 4 von 4

Warum gibt der Mensch in der Wallet seines Handys frei und nicht im Chat des Agenten?

- Handys haben größere Bildschirme.
- **✓** Der Agent kann nicht auf den Bildschirm der Wallet zeichnen, und nur das Handy hat den Schlüssel, der die Mandate signiert.
- UCP verlangt ein Handy.

**Warum:** AP2 nennt das eine vertrauenswürdige Oberfläche. Der Mensch sieht die Mandate so, wie sie ausgestellt wurden, und die Signatur kommt von einem Schlüssel, den der Agent nie hat.

## Was du jetzt kannst

- Eine UCP-Checkout-Session vom Profil bis completed verfolgen.
- Erklären, wie AP2-Mandate eine Zahlung an die signierte Zustimmung eines Menschen binden.
- Idempotency-Keys nutzen, damit Retries sicher sind.
- In zwei Sätzen sagen, was did:wba und signierte Anfragen von ANP hinzufügen.

---

[Zurück zu Agentensysteme und Standards](README.md) · [Weiter: Orchestrierung: Supervisor, Fan-out, Subgraphen](05-orchestration.md)
