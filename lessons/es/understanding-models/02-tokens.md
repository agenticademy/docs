# Tokens

Entender los modelos · Capítulo 2 de 8 · 14 minutos

Los modelos leen y escriben texto en forma de tokens, cada familia de modelos divide el texto a su manera, y tanto los precios como los límites de contexto se cuentan en tokens.

## Objetivos

- Divide una oración con los tokenizadores de seis familias de modelos.
- Cambia a español, japonés y código, y compara los conteos.
- Supera la comprobación.

## La lectura

### El texto se convierte en tokens

En el capítulo anterior conociste muchos tipos de modelos. Los que trabajan con texto empiezan todos de la misma manera. Un tokenizador, un pequeño programa que viene con cada modelo, corta el texto en tokens: trozos que a menudo son una palabra entera con el espacio que va antes, y a veces parte de una palabra, un signo o un dígito. Luego cada token se cambia por un número, su ID de token. El modelo lee solo estos números. Cuando escribe, devuelve IDs de token, y el tokenizador los convierte en texto.

### Cada familia tiene su propio vocabulario

La lista de trozos que conoce un tokenizador es su vocabulario. Se arma a partir de una gran muestra de texto antes de entrenar el modelo: los trozos que aparecen a menudo ahí se vuelven un token cada uno. Cada familia de modelos, como GPT, Llama o Qwen, tiene su propio tokenizador, y una nueva generación puede recibir uno nuevo. Los seis del laboratorio conocen entre 32.768 y 262.144 trozos.

Una palabra que no está en el vocabulario se arma con trozos más pequeños. Al final de todo están los bytes, los pequeños códigos que usa una computadora para guardar cada carácter. Un carácter japonés o un emoji ocupa tres o cuatro bytes, así que un tokenizador que no lo tiene necesita varios tokens para un solo carácter.

### Por qué importa el conteo

Los proveedores ponen precio a sus modelos por millón de tokens: los tokens que envías, llamados entrada, y los tokens que escribe el modelo, llamados salida. La ventana de contexto de un modelo, el máximo de texto que puede recibir de una vez, también se cuenta en tokens. Un texto que necesita más tokens cuesta más y llena antes la ventana. En el laboratorio, la misma solicitud ocupa más tokens en español y en japonés que en inglés en todas las familias, y el código y los emoji meten menos caracteres en cada token que el inglés.

### Qué hay en el laboratorio

Seis tokenizadores: el de OpenAI para GPT-4o, un modelo cerrado, y los de cinco familias de pesos abiertos, cuyos archivos de modelo están publicados: Gemma 4 de Google, Llama 3 de Meta, Qwen3, DeepSeek-V3 y Mistral v3. Sus divisiones se grabaron con sus archivos de tokenizador reales. El último paso es opcional: descarga los archivos que elijas, mostrando antes su tamaño, y divide tu propio texto en esta página.

## Pruébalo: Laboratorio de tokenizadores

Divide una oración con seis tokenizadores, luego cambia el texto y mira cómo cambian los conteos.

https://agenticademy.org/es/paths/understanding-models/02-tokens

## Comprobación

Esta palabra alemana significa “mensaje de confirmación de reserva de mesa”. GPT-4o convierte la oración en inglés del laboratorio en 10 tokens, más o menos uno por palabra. ¿En cuántos convierte esta única palabra?

La palabra:

```
Tischreservierungsbestätigungsnachricht

1 palabra, 39 letras
Tokens de GPT-4o: ?
```

- 1, porque es una sola palabra.
- **✓** 9, porque se arma con trozos más largos que están en el vocabulario.
- 39, uno por cada letra.
- Ninguno, porque un modelo omite las palabras que nunca vio.

**Por qué:** GPT-4o no tiene un token único para esta palabra poco común, así que su tokenizador la arma con 9 trozos que conoce: T, isch, reserv, ierungs, best, ät, igungs, nach, richt. No es 1, porque solo las palabras comunes tienen un token propio. No es 39, porque las secuencias de letras que aparecen a menudo se guardan como un solo trozo. Y no se omite nada: cualquier texto se puede armar con trozos, hasta llegar a bytes sueltos. Otras familias la dividen de otra manera: Mistral v3 necesita 13 tokens.

## Lo que ya puedes hacer

- Explicar qué son un tokenizador, un token y un vocabulario.
- Decir por qué el mismo texto puede ser un número distinto de tokens en dos familias de modelos.
- Explicar por qué los precios y las ventanas de contexto dependen del conteo de tokens, y por qué otros idiomas, el código y los emoji suelen costar más.

---

[Volver a Entender los modelos](README.md) · [Siguiente: Elegir el siguiente token](03-next-token.md)
