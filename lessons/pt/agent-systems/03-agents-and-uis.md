# Agentes e interfaces: AG-UI e A2UI

Sistemas de agentes e padrões · Capítulo 3 de 8 · 25 minutos

Transmitir o trabalho de um agente a um app com AG-UI, e deixar um agente descrever componentes de interface seguros com A2UI.

## Objetivos

- Acompanhe o fluxo de eventos AG-UI de uma execução até o fim.
- Confirme o reembolso por meio de uma ferramenta de front-end.
- Injete um componente fora do catálogo A2UI e veja-o ser recusado.
- Passe na verificação.

## A leitura

### AG-UI: o trabalho do agente como um fluxo

O MCP conecta um agente a ferramentas, e o A2A a outros agentes. O AG-UI (Agent–User Interaction protocol) o conecta ao app que uma pessoa usa. O app envia um RunAgentInput (id da thread, id da execução, mensagens, estado e as ferramentas que o app oferece) e lê de volta um fluxo de eventos, aqui como server-sent events.

Uma execução começa com RUN\_STARTED e termina com RUN\_FINISHED ou RUN\_ERROR. O texto chega como TEXT\_MESSAGE\_START, depois fragmentos TEXT\_MESSAGE\_CONTENT, depois TEXT\_MESSAGE\_END. As chamadas de ferramenta chegam do mesmo jeito: TOOL\_CALL\_START, TOOL\_CALL\_ARGS, TOOL\_CALL\_END, e TOOL\_CALL\_RESULT para ferramentas que o agente executou. O estado compartilhado se move como STATE\_SNAPSHOT, o estado inteiro, ou STATE\_DELTA, uma lista de operações JSON Patch. O cliente oficial, @ag-ui/client 1.0.2, confere o schema e a ordem de cada evento antes de aplicá-lo.

### Ferramentas de front-end

Algumas ferramentas pertencem ao app: um botão de confirmar, um seletor de data. O app as lista em RunAgentInput.tools. Quando o agente chama uma delas, a execução termina com essa chamada pendente; o app executa a ferramenta, aqui perguntando ao cliente, e inicia a próxima execução com o resultado como uma mensagem de ferramenta. A resposta da pessoa passa a fazer parte da conversa, e o agente decide o que fazer com ela.

### A2UI: interfaces como dados

O A2UI (protocolo v0.9) permite que um agente descreva um pedaço de interface sem enviar código. createSurface abre uma superfície e indica o catálogo que ela usa; updateComponents envia uma lista plana de componentes que se referem uns aos outros por id; updateDataModel envia os valores aos quais eles se vinculam. O app renderiza apenas componentes do seu próprio catálogo, aqui sete tipos: Card, Column, Row, List, Text, Button e Divider. Com validação estrita, @a2ui/web\_core 0.12.0 recusa um tipo desconhecido e a superfície fica como estava, então um agente, ou um prompt que o enganou, não consegue fazer o app executar código novo.

### O que você faz aqui

Um cliente pergunta ao chat de suporte sobre a cobrança em dobro. Pelo AG-UI, acompanhe o fluxo de eventos e depois confirme o reembolso no card que o app desenha. Depois mude o lab para A2UI, onde o agente monta um card de reembolso a partir de mensagens. Ative Interceptar, retenha a mensagem updateComponents e adicione um componente Script: o web\_core o recusa.

## Experimente: Um agente monta uma tela

Quatro passos, uma ação cada, e depois dois opcionais. O lab abaixo dos passos desenha cada mensagem como uma linha, de cima para baixo. Enquanto um lab A2UI está aberto, uma árvore abaixo do lab mostra os componentes que o agente enviou, ligados por id.

https://agenticademy.org/pt/paths/agent-systems/03-agents-and-uis

## Verificação

### Pergunta 1 de 4

O que é uma ferramenta de front-end no AG-UI?

- **✓** Uma ferramenta que o app executa, como uma caixa de confirmação; o resultado volta para o agente na próxima execução.
- Uma ferramenta que desenha gráficos no servidor.
- Uma extensão de navegador que o agente instala.

**Por quê:** O agente a pede como qualquer outra ferramenta, mas o app a declara e a executa, então uma pessoa pode participar do passo. A execução termina com a chamada pendente; o resultado inicia a próxima execução.

### Pergunta 2 de 4

Por que o A2UI renderiza a partir de um catálogo em vez de executar o markup do agente?

- É mais rápido.
- **✓** O app decide quais componentes existem, então um agente, ou um texto que lhe deram, não consegue fazer o app executar código novo.
- O HTML exige catálogos.

**Por quê:** Descrever a UI como dados mantém o app no controle. Com validação estrita, um tipo de componente fora do catálogo é recusado, seja o que for que o agente envie.

### Pergunta 3 de 4

O que um evento STATE\_DELTA do AG-UI carrega?

- Uma cópia completa do estado.
- **✓** Operações JSON Patch a aplicar no estado compartilhado.
- O uso de tokens do modelo.

**Por quê:** Os deltas mantêm as atualizações pequenas. Se um patch aponta para um caminho que não existe, o cliente não consegue aplicá-lo e mantém o estado anterior; um STATE\_SNAPSHOT o põe de volta em sincronia.

### Pergunta 4 de 4

O cliente recebe TEXT\_MESSAGE\_CONTENT para um messageId que nunca teve um TEXT\_MESSAGE\_START. O que acontece?

- O texto é mostrado mesmo assim.
- **✓** O @ag-ui/client rejeita o evento por estar fora de ordem e para a execução.
- O agente o reenvia.

**Por quê:** O cliente confere a ordem dos eventos, além do schema deles. Conteúdo para uma mensagem que nunca começou é um erro de protocolo, então a execução para.

## O que você já sabe fazer

- Ler um fluxo de eventos AG-UI e dizer o que cada evento faz.
- Usar uma ferramenta de front-end para colocar uma pessoa dentro da execução de um agente.
- Descrever uma superfície A2UI: createSurface, updateComponents e updateDataModel.
- Explicar como o catálogo do A2UI protege um app de uma UI feita por um agente.

---

[Voltar para Sistemas de agentes e padrões](README.md) · [Próximo: Agentes que pagam: UCP e AP2](04-agent-commerce.md)
