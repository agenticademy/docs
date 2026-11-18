# Outils et appels d'outils

Construisez vos premiers agents · Chapitre 5 sur 8 · 20 minutes

Définir des outils avec zod, les lier à un modèle et traiter les appels d'outils que fait le modèle.

## Objectifs

- Ajoutez un champ décrit au schéma d'un outil et regardez son JSON Schema changer.
- Envoyez des arguments invalides et voyez la validation les rejeter.
- Réussissez la vérification.

## La lecture

### Un outil est une fonction avec un schéma

tool(fn, { name, description, schema }), de @langchain/core/tools, transforme une fonction async en outil. Le schéma est un objet zod : z.object({ order\_id: z.string().describe("The order number") }). Il indique quels arguments l'outil prend, leurs types, et lesquels sont facultatifs.

Le schéma sert deux fois. LangChain le convertit en JSON Schema, que lit le modèle. Et votre code s'en sert pour vérifier les arguments envoyés par le modèle avant que votre fonction s'exécute.

### Ce que voit le modèle

model.bindTools(tools) renvoie un modèle qui envoie le nom, la description et le JSON Schema de chaque outil à chaque appel. La fonction elle-même ne quitte jamais votre code. Le modèle ne sait que ce que ces trois éléments lui disent : les descriptions sont donc des prompts. Écrivez-les pour le modèle, et décrivez chaque champ avec .describe().

### Un appel d'outil et sa réponse

Quand le modèle veut un outil, sa réponse est un AIMessage avec des tool\_calls : chaque appel a un name, des args et un id. Votre code exécute l'outil et répond avec un ToolMessage qui porte le même tool\_call\_id. Un modèle peut demander plusieurs appels à la fois : c'est l'id qui associe chaque résultat à son appel.

Passez l'appel d'outil entier à tool.invoke(). L'outil valide les args selon son schéma, exécute votre fonction et renvoie le ToolMessage pour vous. Si les args ne correspondent pas, il lève ToolInputParsingException et votre fonction ne s'exécute jamais. Une boucle d'agent intercepte l'erreur et la renvoie sous forme de ToolMessage avec le status "error", pour que le modèle puisse corriger son appel. Au chapitre 6, ToolNode le fait pour vous.

## Essayez : De zod à l'appel d'outil

Ajoutez un champ au schéma d'un outil et voyez ce que lit le modèle. Puis faites passer l'appel du modèle par la vraie validation, une fois avec des arguments valides et une fois avec des arguments que vous choisissez.

https://agenticademy.org/fr/paths/first-agents/05-tools-and-tool-calls

## Vérification

### Question 1 sur 3

Que reçoit le modèle au sujet d'un outil lié ?

- Le code source de l'outil.
- **✓** Son nom, sa description et son JSON Schema.
- Seulement son nom.

**Pourquoi:** bindTools convertit chaque outil au format de son fournisseur : nom, description et schéma d'entrée. La fonction elle-même ne quitte jamais votre code.

### Question 2 sur 3

Qu'est-ce qui relie un ToolMessage à l'appel auquel il répond ?

- Le nom de l'outil.
- **✓** Le tool\_call\_id, égal à l'id de l'appel dans l'AIMessage.
- Leur position dans la liste.

**Pourquoi:** Un modèle peut demander plusieurs appels à la fois, même du même outil. L'id associe chaque résultat à son appel.

### Question 3 sur 3

Le modèle envoie order\_id sous forme de nombre, mais le schéma indique z.string(). Que se passe-t-il ?

- L'outil s'exécute et le convertit.
- **✓** La validation échoue avant que votre fonction s'exécute, et l'erreur peut retourner au modèle.
- Le modèle est réentraîné.

**Pourquoi:** Le schéma est appliqué dans votre code. Une boucle peut renvoyer l'erreur sous forme de ToolMessage pour que le modèle réessaie.

## Ce que vous savez faire maintenant

- Définir un outil avec tool() et un schéma zod, avec des descriptions utiles.
- Lier des outils à un modèle et lire ses tool\_calls.
- Répondre à un appel d'outil avec un ToolMessage, et traiter les arguments invalides.

---

[Retour à Construisez vos premiers agents](README.md) · [Suivant : La boucle de l'agent : un agent ReAct minimal](06-the-agent-loop.md)
