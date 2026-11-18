# Agents that pay: UCP and AP2

Agent systems and standards · Chapter 4 of 8 · 25 minutes

How an agent checks out with a merchant over UCP, and how AP2 mandates bind the payment to what the person signed.

## Goals

- Create and complete a checkout session.
- Sign the mandates on the phone.
- Change the amount after signing and see verification fail.
- Pass the check.

## The read

### UCP: the checkout

UCP (Universal Commerce Protocol, version 2026-08-25) is how a shopping agent, the platform, buys from a merchant, the business. The business publishes a profile at /.well-known/ucp: its version, its capabilities such as checkout, its payment handlers and its REST endpoint. The agent reads it, then creates a checkout session with POST /checkout-sessions, updates it with PUT until the status is ready\_for\_complete, and finishes with POST …/complete.

Every call carries a UCP-Agent header pointing at the platform's own profile, so the business can check what the platform supports. Every write carries an Idempotency-Key: if a timeout makes the agent send the same request again, the business answers with the first result instead of acting twice. A double charge like Halden's order 48213 is exactly what it prevents.

### AP2: the person's signature

UCP alone trusts whoever holds the session. AP2 (Agent Payments Protocol) adds proof of consent. The agent's provider issues two mandates as SD-JWT verifiable credentials: a checkout mandate holding a hash of the exact checkout Halden signed, and a payment mandate holding the amount, the payee and the card. The person reviews them on a screen the agent cannot draw on, their phone's wallet, and the phone's key signs a key-binding JWT on each.

Halden checks the checkout mandate against the checkout it signed. The card wallet checks the payment mandate before it issues a payment token for that amount, and the processor checks the token before it charges. Change the cart or the amount after the person signed and a hash or a signature no longer matches, whatever the agent says.

Here the person is present. In AP2's autonomous mode they are not: they approve open mandates in advance, with limits such as a maximum price, and the agent signs the final mandates within them.

### ANP, in two sentences

ANP (Agent Network Protocol) is for agents meeting on the open web: each agent has a did:wba identifier whose DID document, served from its own domain, lists its public keys. Every request is signed with HTTP Message Signatures, so the other side knows which agent sent it without an account. The ANP labs in the Go further step run it.

### What you do here

A shopping agent buys Halden's Ridge 2 tent. Follow the checkout, approve on the phone, then run it again with Intercept on and raise the amount after signing.

## Try it: A chain of signatures

Five steps, then two optional ones. Under the steps, the chain adds each link of the purchase as the purchase reaches it, with who checks it. Under the chain, the lab draws every message as a row, from the top down.

https://agenticademy.org/en/paths/agent-systems/04-agent-commerce

## Check

### Question 1 of 4

What does an AP2 checkout mandate bind?

- The agent's name.
- **✓** A hash of the exact checkout, with its cart and total, that the person approved.
- The merchant's logo.

**Why:** The mandate is signed proof of what the person agreed to buy. A checkout that differs has a different hash, so it no longer matches.

### Question 2 of 4

The agent raises the amount after the person signed. Where does it fail?

- Nowhere, if the agent explains why.
- **✓** At verification: the card wallet, Halden or the processor finds that a signature or hash no longer matches.
- Only at the bank, days later.

**Why:** Each party checks the signatures against the values in front of it. The agent cannot talk its way past a hash.

### Question 3 of 4

What does UCP's Idempotency-Key prevent?

- Prompt injection.
- **✓** A retried request creating a second order or charge.
- Expired cards.

**Why:** A network retry repeats a request. With the same key, the business returns the first result instead of acting twice: the double charge in Halden's story is exactly what it prevents.

### Question 4 of 4

Why does the person approve on their phone's wallet rather than in the agent's chat?

- Phones have bigger screens.
- **✓** The agent cannot draw on the wallet's screen, and only the phone holds the key that signs the mandates.
- UCP requires a phone.

**Why:** AP2 calls it a trusted surface. The person sees the mandates as issued, and the signature comes from a key the agent never holds.

## What you can do now

- Follow a UCP checkout session from the profile to completed.
- Explain how AP2 mandates bind a payment to a person's signed consent.
- Use idempotency keys to make retries safe.
- Say in two sentences what ANP's did:wba and signed requests add.

---

[Back to Agent systems and standards](README.md) · [Next: Orchestration: supervisors, fan-out, subgraphs](05-orchestration.md)
