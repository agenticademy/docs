# Herramientas y llamadas a herramientas

Construye tus primeros agentes · Capítulo 5 de 8 · 20 minutos

Definir herramientas con zod, vincularlas a un modelo y manejar las llamadas a herramientas que hace el modelo.

## Objetivos

- Agrega un campo descrito al esquema de una herramienta y mira cómo cambia su JSON Schema.
- Envía argumentos inválidos y mira cómo la validación los rechaza.
- Supera la comprobación.

## La lectura

### Una herramienta es una función con un esquema

tool(fn, { name, description, schema }) de @langchain/core/tools convierte una función async en una herramienta. El esquema es un objeto de zod: z.object({ order\_id: z.string().describe("The order number") }). Dice qué argumentos recibe la herramienta, sus tipos y cuáles son opcionales.

El esquema sirve dos veces. LangChain lo convierte a JSON Schema, que es lo que lee el modelo. Y tu código lo usa para revisar los argumentos que envía el modelo antes de que se ejecute tu función.

### Lo que ve el modelo

model.bindTools(tools) devuelve un modelo que envía el nombre, la descripción y el JSON Schema de cada herramienta en cada llamada. La función en sí nunca sale de tu código. El modelo solo sabe lo que le dicen esos tres, así que las descripciones son prompts: escríbelas para el modelo y describe cada campo con .describe().

### Una llamada a herramienta y su respuesta

Cuando el modelo quiere una herramienta, su respuesta es un AIMessage con tool\_calls: cada llamada tiene un name, unos args y un id. Tu código ejecuta la herramienta y responde con un ToolMessage que lleva el mismo tool\_call\_id. Un modelo puede pedir varias llamadas a la vez, así que el id es lo que empareja cada resultado con su llamada.

Pasa la llamada a herramienta entera a tool.invoke(). La herramienta valida los args contra su esquema, ejecuta tu función y te devuelve el ToolMessage. Si los args no coinciden, lanza ToolInputParsingException y tu función nunca se ejecuta. Un ciclo de agente atrapa el error y lo devuelve como un ToolMessage con status "error", para que el modelo pueda corregir su llamada. En el capítulo 6, ToolNode hace esto por ti.

## Pruébalo: De zod a la llamada a herramienta

Agrega un campo al esquema de una herramienta y mira lo que lee el modelo. Luego pasa la llamada del modelo por la validación real, una vez con argumentos válidos y otra con argumentos que tú eliges.

https://agenticademy.org/es/paths/first-agents/05-tools-and-tool-calls

## Comprobación

### Pregunta 1 de 3

¿Qué recibe el modelo sobre una herramienta vinculada?

- El código fuente de la herramienta.
- **✓** Su nombre, su descripción y su JSON Schema.
- Solo su nombre.

**Por qué:** bindTools convierte cada herramienta al formato de su proveedor: nombre, descripción y el esquema de entrada. La función en sí nunca sale de tu código.

### Pregunta 2 de 3

¿Qué vincula un ToolMessage con la llamada a la que responde?

- El nombre de la herramienta.
- **✓** El tool\_call\_id, igual al id de la llamada en el AIMessage.
- Su posición en la lista.

**Por qué:** Un modelo puede pedir varias llamadas a la vez, incluso a la misma herramienta. El id empareja cada resultado con su llamada.

### Pregunta 3 de 3

El modelo envía order\_id como number, pero el esquema dice z.string(). ¿Qué pasa?

- La herramienta se ejecuta y lo convierte.
- **✓** La validación falla antes de que se ejecute tu función, y el error puede volver al modelo.
- El modelo se reentrena.

**Por qué:** El esquema se aplica en tu código. Un ciclo puede devolver el error como un ToolMessage para que el modelo lo intente de nuevo.

## Lo que ya puedes hacer

- Definir una herramienta con tool() y un esquema de zod, con descripciones útiles.
- Vincular herramientas a un modelo y leer sus tool\_calls.
- Responder una llamada a herramienta con un ToolMessage, y manejar argumentos inválidos.

---

[Volver a Construye tus primeros agentes](README.md) · [Siguiente: El ciclo del agente: un agente ReAct mínimo](06-the-agent-loop.md)
