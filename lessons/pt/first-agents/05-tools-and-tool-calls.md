# Ferramentas e chamadas de ferramenta

Construa seus primeiros agentes · Capítulo 5 de 8 · 20 minutos

Definir ferramentas com zod, vinculá-las a um modelo e tratar as chamadas de ferramenta que o modelo faz.

## Objetivos

- Adicione um campo descrito ao schema de uma ferramenta e veja o JSON Schema mudar.
- Envie argumentos inválidos e veja a validação rejeitá-los.
- Passe na verificação.

## A leitura

### Uma ferramenta é uma função com um schema

tool(fn, { name, description, schema }), de @langchain/core/tools, transforma uma função async em uma ferramenta. O schema é um objeto zod: z.object({ order\_id: z.string().describe("The order number") }). Ele diz quais argumentos a ferramenta recebe, seus tipos e quais são opcionais.

O schema serve duas vezes. O LangChain o converte em JSON Schema, que é o que o modelo lê. E seu código o usa para verificar os argumentos que o modelo envia antes que sua função rode.

### O que o modelo vê

model.bindTools(tools) devolve um modelo que envia o nome, a descrição e o JSON Schema de cada ferramenta em toda chamada. A função em si nunca sai do seu código. O modelo só sabe o que esses três lhe dizem, então as descrições são prompts: escreva-as para o modelo e descreva cada campo com .describe().

### Uma chamada de ferramenta e sua resposta

Quando o modelo quer uma ferramenta, sua resposta é um AIMessage com tool\_calls: cada chamada tem um name, args e um id. Seu código executa a ferramenta e responde com um ToolMessage que traz o mesmo tool\_call\_id. Um modelo pode pedir várias chamadas de uma vez, então é o id que liga cada resultado à sua chamada.

Passe a chamada de ferramenta inteira para tool.invoke(). A ferramenta valida os args contra o seu schema, executa sua função e devolve o ToolMessage para você. Se os args não batem, ela lança ToolInputParsingException e sua função nunca roda. Um loop de agente captura o erro e o envia de volta como um ToolMessage com status "error", para que o modelo possa corrigir a chamada. No capítulo 6, o ToolNode faz isso por você.

## Experimente: Do zod à chamada de ferramenta

Adicione um campo ao schema de uma ferramenta e veja o que o modelo lê. Depois passe a chamada do modelo pela validação real, uma vez com argumentos válidos e outra com argumentos que você escolhe.

https://agenticademy.org/pt/paths/first-agents/05-tools-and-tool-calls

## Verificação

### Pergunta 1 de 3

O que o modelo recebe sobre uma ferramenta vinculada?

- O código-fonte da ferramenta.
- **✓** Seu nome, sua descrição e seu JSON Schema.
- Só o nome.

**Por quê:** bindTools converte cada ferramenta para o formato do seu provedor: nome, descrição e o schema de entrada. A função em si nunca sai do seu código.

### Pergunta 2 de 3

O que liga um ToolMessage à chamada que ele responde?

- O nome da ferramenta.
- **✓** O tool\_call\_id, igual ao id da chamada no AIMessage.
- A posição na lista.

**Por quê:** Um modelo pode pedir várias chamadas de uma vez, até da mesma ferramenta. O id liga cada resultado à sua chamada.

### Pergunta 3 de 3

O modelo envia order\_id como number, mas o schema diz z.string(). O que acontece?

- A ferramenta roda e o converte.
- **✓** A validação falha antes que sua função rode, e o erro pode voltar ao modelo.
- O modelo é retreinado.

**Por quê:** O schema é aplicado no seu código. Um loop pode devolver o erro como um ToolMessage para que o modelo tente de novo.

## O que você já sabe fazer

- Definir uma ferramenta com tool() e um schema zod, com descrições úteis.
- Vincular ferramentas a um modelo e ler seus tool\_calls.
- Responder a uma chamada de ferramenta com um ToolMessage e tratar argumentos inválidos.

---

[Voltar para Construa seus primeiros agentes](README.md) · [Próximo: O loop do agente: um agente ReAct mínimo](06-the-agent-loop.md)
