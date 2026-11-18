# Una llamada al modelo

Construye tus primeros agentes · Capítulo 2 de 8 · 15 minutos

Llamar a un modelo de chat con LangChain.js, leer el AIMessage que devuelve y usar tu propia clave de forma segura.

## Objetivos

- Ejecuta la llamada con el modelo con guion.
- Encuentra los conteos de tokens en la respuesta.
- Predice lo que imprime el código.

## La lectura

### Una interfaz, muchos proveedores

Cada paquete de proveedor tiene una clase de modelo de chat, como ChatAnthropic o ChatOpenAI. Comparten una interfaz: invoke(messages) envía una lista de mensajes y devuelve un AIMessage. También puedes pasar un string simple, que LangChain envuelve en un HumanMessage. stream(messages) devuelve la misma respuesta en fragmentos a medida que llegan, para que puedas mostrar texto mientras el modelo todavía escribe.

Las opciones van al constructor: model (el nombre del modelo en ese proveedor), temperature (0 da la redacción más predecible; valores más altos varían más) y maxTokens (la respuesta más larga que vas a pagar). ChatOpenAI también acepta configuration.baseURL, así que la misma clase habla con cualquier servicio compatible con OpenAI: OpenRouter, el router de Hugging Face o el catálogo de API de NVIDIA.

### Qué vuelve

El AIMessage tiene content (el texto, para una respuesta simple), tool\_calls (vacío hasta el capítulo 5), usage\_metadata y response\_metadata. usage\_metadata cuenta tokens, los trozos de texto que un modelo lee y escribe: input\_tokens para lo que enviaste, output\_tokens para la respuesta. Los proveedores cobran por tokens, y la entrada crece con cada mensaje que envías, así que este es el número que hay que vigilar. response\_metadata contiene lo que agrega el proveedor, como el nombre del modelo y por qué se detuvo.

### Claves

Una clave de API es una contraseña de una cuenta paga. Mantenla fuera del código y de los repositorios: léela de una variable de entorno en tu máquina o servidor. En esta página, una clave que conectas se queda en esta pestaña y va de tu navegador directo al proveedor. Eso está bien para aprender, pero una app pública debería llamar a los modelos desde un servidor, para que su clave nunca se envíe a los visitantes. Existen planes gratuitos, con límites de frecuencia.

## Pruébalo: Una petición y su respuesta

Ejecuta la llamada y abre el AIMessage que vuelve. Conecta tu propio modelo si quieres ver una respuesta real.

https://agenticademy.org/es/paths/first-agents/02-one-model-call

## Comprobación

El modelo es un modelo de chat que responde "Hello." e informa su uso. ¿Qué imprime esto?

Código:

```
const reply = await model.invoke("Say hello.");
console.log(typeof reply.content, reply.usage_metadata.output_tokens > 0);
```

- **✓** string true
- object true
- string undefined
- Lanza un error: invoke necesita un array de mensajes.

**Por qué:** invoke también acepta un string simple, que LangChain envuelve en un HumanMessage. Para una respuesta de texto, content es un string, y usage\_metadata cuenta los tokens de salida.

## Lo que ya puedes hacer

- Llamar a un modelo de chat con invoke() y stream().
- Leer el contenido y el uso de tokens de un AIMessage.
- Apuntar ChatOpenAI a un servicio compatible con OpenAI con baseURL.
- Explicar dónde debería y dónde no debería estar una clave de API.

---

[Volver a Construye tus primeros agentes](README.md) · [Siguiente: Mensajes y estado](03-messages-and-state.md)
