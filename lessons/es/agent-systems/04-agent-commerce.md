# Agentes que pagan: UCP y AP2

Sistemas de agentes y estándares · Capítulo 4 de 8 · 25 minutos

Cómo un agente completa una compra con un comercio por UCP, y cómo los mandatos de AP2 atan el pago a lo que firmó la persona.

## Objetivos

- Crea y completa una sesión de checkout.
- Firma los mandatos en el teléfono.
- Cambia el monto después de firmar y mira cómo falla la verificación.
- Supera la comprobación.

## La lectura

### UCP: el checkout

UCP (Universal Commerce Protocol, versión 2026-08-25) es la forma en que un agente de compras, la plataforma, le compra a un comercio, el negocio. El negocio publica un perfil en /.well-known/ucp: su versión, sus capacidades como checkout, sus payment handlers y su endpoint REST. El agente lo lee, luego crea una sesión de checkout con POST /checkout-sessions, la actualiza con PUT hasta que el estado es ready\_for\_complete, y termina con POST …/complete.

Cada llamada lleva un header UCP-Agent que apunta al perfil propio de la plataforma, para que el negocio pueda revisar qué soporta la plataforma. Cada escritura lleva un Idempotency-Key: si un timeout hace que el agente envíe de nuevo la misma petición, el negocio responde con el primer resultado en lugar de actuar dos veces. Un cobro doble como el del pedido 48213 de Halden es justo lo que evita.

### AP2: la firma de la persona

UCP por sí solo confía en quien tenga la sesión. AP2 (Agent Payments Protocol) agrega una prueba de consentimiento. El proveedor del agente emite dos mandatos como credenciales verificables SD-JWT: un mandato de checkout con un hash del checkout exacto que firmó Halden, y un mandato de pago con el monto, el beneficiario y la tarjeta. La persona los revisa en una pantalla en la que el agente no puede dibujar, la billetera de su teléfono, y la clave del teléfono firma un key-binding JWT en cada uno.

Halden verifica el mandato de checkout contra el checkout que firmó. La billetera de tarjetas verifica el mandato de pago antes de emitir un token de pago por ese monto, y el procesador verifica el token antes de cobrar. Cambia el carrito o el monto después de que la persona firmó y un hash o una firma deja de coincidir, diga lo que diga el agente.

Aquí la persona está presente. En el modo autónomo de AP2 no lo está: aprueba de antemano mandatos abiertos, con límites como un precio máximo, y el agente firma los mandatos finales dentro de ellos.

### ANP, en dos oraciones

ANP (Agent Network Protocol) es para agentes que se encuentran en la web abierta: cada agente tiene un identificador did:wba cuyo documento DID, servido desde su propio dominio, lista sus claves públicas. Cada petición se firma con HTTP Message Signatures, así que el otro lado sabe qué agente la envió sin necesidad de una cuenta. Los laboratorios ANP del paso Ve más allá lo ejecutan.

### Qué haces aquí

Un agente de compras compra la carpa Ridge 2 de Halden. Sigue el checkout, aprueba en el teléfono y luego ejecútalo de nuevo con Interceptar activado y sube el monto después de firmar.

## Pruébalo: Una cadena de firmas

Cinco pasos y luego dos opcionales. Debajo de los pasos, la cadena agrega cada eslabón de la compra cuando la compra llega a él, con quién lo verifica. Debajo de la cadena, el laboratorio dibuja cada mensaje como una fila, de arriba abajo.

https://agenticademy.org/es/paths/agent-systems/04-agent-commerce

## Comprobación

### Pregunta 1 de 4

¿Qué ata un mandato de checkout de AP2?

- El nombre del agente.
- **✓** Un hash del checkout exacto, con su carrito y su total, que aprobó la persona.
- El logo del comercio.

**Por qué:** El mandato es una prueba firmada de lo que la persona aceptó comprar. Un checkout distinto tiene otro hash, así que deja de coincidir.

### Pregunta 2 de 4

El agente sube el monto después de que la persona firmó. ¿Dónde falla?

- En ningún lado, si el agente explica por qué.
- **✓** En la verificación: la billetera de tarjetas, Halden o el procesador ven que una firma o un hash ya no coincide.
- Solo en el banco, días después.

**Por qué:** Cada parte verifica las firmas contra los valores que tiene delante. El agente no puede convencer a un hash.

### Pregunta 3 de 4

¿Qué evita el Idempotency-Key de UCP?

- La inyección de prompts.
- **✓** Que una petición reintentada cree un segundo pedido o cobro.
- Las tarjetas vencidas.

**Por qué:** Un reintento de red repite una petición. Con la misma clave, el negocio devuelve el primer resultado en lugar de actuar dos veces: el cobro doble de la historia de Halden es justo lo que evita.

### Pregunta 4 de 4

¿Por qué la persona aprueba en la billetera de su teléfono y no en el chat del agente?

- Los teléfonos tienen pantallas más grandes.
- **✓** El agente no puede dibujar en la pantalla de la billetera, y solo el teléfono tiene la clave que firma los mandatos.
- UCP exige un teléfono.

**Por qué:** AP2 lo llama una superficie de confianza. La persona ve los mandatos tal como se emitieron, y la firma viene de una clave que el agente nunca tiene.

## Lo que ya puedes hacer

- Seguir una sesión de checkout de UCP desde el perfil hasta completed.
- Explicar cómo los mandatos de AP2 atan un pago al consentimiento firmado de una persona.
- Usar claves de idempotencia para que los reintentos sean seguros.
- Decir en dos oraciones qué agregan el did:wba y las peticiones firmadas de ANP.

---

[Volver a Sistemas de agentes y estándares](README.md) · [Siguiente: Orquestación: supervisores, fan-out, subgrafos](05-orchestration.md)
