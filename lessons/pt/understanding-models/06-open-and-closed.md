# Modelos abertos e fechados

Entendendo os modelos · Capítulo 6 de 8 · 15 minutos

Alguns modelos só podem ser usados pelo serviço do criador, outros compartilham os seus pesos com licenças que vão de rígidas a permissivas, e alguns poucos são de código aberto, com os seus dados e o seu código.

## Objetivos

- Escolha um objetivo, como rodar offline ou lançar um app comercial.
- Verifique os modelos nível por nível e veja quais atendem a esse objetivo.
- Abra uma licença e leia os seus termos principais.
- Passe na verificação.

## A leitura

### Fechado: um serviço que você chama

No capítulo 4, os modelos abertos publicavam os seus pesos, os números que um modelo aprendeu, e os fechados não. Um modelo fechado roda nos computadores do criador. Você chega a ele pelo app do criador ou pela sua API, um serviço para o qual o seu programa envia texto, e paga por token segundo os termos de serviço do criador. Claude, GPT, Gemini e Grok funcionam assim.

### Pesos abertos, com uma licença

Um modelo de pesos abertos deixa você baixar os seus pesos. Você pode rodá-lo no seu próprio computador sem conexão com a internet e fazer ajuste fino: treiná-lo mais com os seus próprios exemplos. Uma licença vem com os pesos, o texto jurídico que diz o que você pode fazer com eles.

Uma licença permissiva, como Apache-2.0 ou MIT, permite quase qualquer uso, inclusive comercial, desde que você mantenha o seu aviso. Uma licença restritiva acrescenta condições, como um acordo separado acima de um número de usuários ou de um valor de receita, um nome a exibir ou usos que não são permitidos. Gemma 4, gpt-oss e DeepSeek-V4.1-Flash têm licenças permissivas; Llama 4 e Kimi K3 têm licenças restritivas.

### Código aberto

Pesos abertos não são código aberto. A Open Source Initiative (OSI), que mantém a definição de software de código aberto, escreveu a Open Source AI Definition 1.0. Ela pede três coisas sob termos abertos: os pesos, o código que treina e roda o modelo, e informações sobre os dados, o suficiente sobre os dados de treinamento para que uma pessoa qualificada construa um modelo parecido. Poucos modelos a atendem. O Olmo 3, que você acompanhou no capítulo 4, publica os três.

### Como obter e rodar

Os modelos abertos são compartilhados em plataformas como o Hugging Face, um site que guarda os arquivos, a ficha de modelo e a licença de cada modelo. Alguns têm acesso restrito: você aceita a licença, e às vezes espera a aprovação do criador, antes de o download começar. No seu computador, apps os rodam a partir de arquivos feitos para eles: llama.cpp e Ollama leem arquivos GGUF, o MLX roda modelos em Macs com Apple silicon, e um navegador com WebGPU pode rodar os pequenos, como no capítulo 3. Se um cabe ou não é a pergunta do capítulo 5.

### No visual

Você escolhe um objetivo e verifica modelos reais nível por nível, de fechado a código aberto. Depois abre uma licença. Os fatos vêm das páginas dos modelos e das licenças, em um levantamento de 2026-11-03. É um resumo para aprender, não aconselhamento jurídico.

## Experimente: Matriz de licenças

Escolha um objetivo, verifique os modelos nível por nível de fechado a código aberto, depois abra uma licença e leia os seus termos principais.

https://agenticademy.org/pt/paths/understanding-models/06-open-and-closed

## Verificação

### Pergunta 1 de 3

Você precisa de um modelo em um notebook sem conexão com a internet. Que tipo consegue fazer isso?

- Um modelo fechado, usado pela sua API a partir do notebook.
- **✓** Um modelo de pesos abertos ou de código aberto, baixado antes para o notebook.
- Qualquer modelo, desde que você tenha uma chave de API para ele.
- Nenhum: todo modelo precisa da internet para responder.

**Por quê:** Rodar offline exige os pesos no notebook, e só os modelos de pesos abertos e de código aberto deixam você baixá-los. Um modelo fechado roda nos servidores do criador, então a sua API precisa de conexão, com chave ou sem. Um modelo aberto baixado não precisa de nenhuma, se o notebook tiver a memória para ele.

### Pergunta 2 de 3

Os pesos de um modelo são publicados sob Apache-2.0, mas nada é dito sobre os seus dados de treinamento. Em que nível ele está?

- Fechado, já que nada é dito sobre os seus dados de treinamento.
- Pesos abertos, licença restritiva.
- Código aberto, pela definição da OSI.
- **✓** Pesos abertos, licença permissiva.

**Por quê:** Fechado significa que os pesos ficam com o criador, e estes são publicados, diga-se o que for sobre os dados. Apache-2.0 permite uso comercial sem limites de usuários ou de receita, então a licença é permissiva, não restritiva. Não é código aberto: a definição da OSI também pede informações sobre os dados e o código de treinamento, e uma licença permissiva sozinha não os dá.

### Pergunta 3 de 3

A licença do Llama 4 diz que acima de 700 milhões de usuários ativos mensais você precisa de outra licença da Meta. Uma startup com 20.000 usuários cria um app pago com o Llama 4. O que essa condição significa para ela?

- Ela não pode usar o Llama 4 em um app pago até a Meta lhe dar outra licença.
- Ela precisa pagar à Meta por token para usar o Llama 4, como faria com um modelo fechado.
- **✓** O limite de usuários não se aplica a ela, mas os outros termos da licença já se aplicam.
- Nada na licença se aplica a ela até ter 700 milhões de usuários mensais.

**Por quê:** O limite começa acima de 700 milhões de usuários ativos mensais, então uma startup pequena pode criar um app pago sem outra licença. Os outros termos valem desde o primeiro usuário: a licença do Llama 4 pede que você exiba “Built with Llama” e siga a política de uso da Meta. Pesos abertos não são pagos por token: você mesmo os roda.

## O que você já sabe fazer

- Distinguir modelos fechados, pesos abertos com licenças restritivas ou permissivas, e código aberto.
- Dizer que tipos de modelo servem para um objetivo: um app comercial, uso offline, ajuste fino ou dados de treinamento conhecidos.
- Ler os termos principais de uma licença, como limites de usuários e de receita, antes de construir sobre um modelo.
- Dizer onde os modelos abertos são compartilhados e quais apps os rodam no seu próprio computador.

---

[Voltar para Entendendo os modelos](README.md) · [Próximo: Comparar modelos](07-comparing-models.md)
