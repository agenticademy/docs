# Un harnais de deep agent

Agents avancés · Chapitre 9 sur 10 · 20 minutes

Un deep agent planifie avec une liste de tâches, garde des notes dans des fichiers virtuels, confie des parties d'un travail à des sous-agents avec un contexte neuf et s'arrête avant les outils que vous nommez.

## Objectifs

- Lancez le travail et regardez l'agent écrire et mettre à jour sa liste de tâches.
- Approuvez l'écriture des notes et regardez l'arborescence de fichiers grandir.
- Lancez trois sous-agents en parallèle et comparez la taille de leurs contextes.
- Réussissez la vérification.

## La lecture

### Un harnais pour les longs travaux

Un harnais est le code autour de la boucle du modèle : le prompt, les outils et ce que voit chaque appel. Le paquet open source deepagents de LangChain en est un, construit sur LangGraph. Son annonce de 2025 citait quatre parties : un prompt détaillé, un outil de planification, des sous-agents et un système de fichiers.

### Tâches et fichiers

write\_todos({ todos }) reçoit toute la liste à chaque fois, chaque élément pending, in\_progress ou completed, et la garde dans state.todos. Il ne fait rien d'autre : l'intérêt est que le modèle relit son plan à chaque appel. Deux appels dans un même tour sont refusés. On l'active en ajoutant todoListMiddleware() de LangChain à middleware.

Les outils de fichiers (ls, read\_file, write\_file, edit\_file, glob, grep) travaillent sur state.files, un système de fichiers virtuel dans l'état du graphe. Les sous-agents peuvent y lire les notes, et celles-ci survivent à un résumé des anciens messages.

### Sous-agents

task({ description, subagent\_type }) lance un sous-agent avec un contexte neuf : son propre system prompt et la description, aucun des messages de l'appelant. Il part des fichiers de l'appelant, les fichiers qu'il écrit sont fusionnés en retour, et seul son dernier message revient, comme résultat de l'outil. Les appels d'un même tour s'exécutent en parallèle. Un sous-agent généraliste, avec les outils de l'appelant, est présent par défaut.

### Skills, mémoire et approbations

Un skill est un dossier avec un fichier SKILL.md. Avec skills: ['/skills/'], le prompt liste le nom et la description de chaque skill, et l'agent ne lit le fichier que quand une tâche en a besoin. L'option memory charge des fichiers AGENTS.md dans le prompt. interruptOn, avec un checkpointer, nomme des outils qui mettent l'exécution en pause jusqu'à ce qu'une personne approuve, modifie ou rejette chaque appel. Cette page se met en pause à un point d'arrêt (interruptBefore), parce que interrupt() a besoin d'AsyncLocalStorage.

### Moins d'échafaudage

Chaque partie coûte des tokens à chaque appel. deepagents 1.12 (2026-07-29) a rendu la liste de tâches facultative et a laissé vide son system prompt générique : les modèles actuels ont besoin de moins de guidage. L'article d'Anthropic sur la conception de harnais (2026-03-24) va dans le même sens : à chaque nouveau modèle, retirez les parties dont il n'a plus besoin.

### Dans ce lab

Un harnais recréé rédige une note de synthèse sur les réclamations de remboursement chez Halden, une boutique fictive. Contrairement à deepagents 1.16, il active la liste avec todos: true, montre les skills au chercheur sans liste de skills à lui, garde l'ancienne ligne générique du prompt et n'a pas de mémoire AGENTS.md. L'agent principal s'arrête avant write\_file et task pour que vous puissiez le suivre ; dans votre propre agent, arrêtez-vous avant les outils qui changent des choses en dehors de lui.

## Essayez : Une recherche, une partie à la fois

Lancez le travail et regardez la liste de tâches, laissez l'agent principal écrire ses notes, puis approuvez trois sous-agents et comparez la taille de leurs contextes.

https://agenticademy.org/fr/paths/advanced-agents/09-deep-agent-harness

## Vérification

Le contexte de l'agent principal fait environ 1 400 tokens quand son modèle fait cet appel. Le chercheur lit environ 600 tokens de fichiers, modifie la note et termine par un rapport de 40 tokens. Qu'est-ce qui change dans l'état de l'agent principal ?

Code:

```
const lead = createDeepAgent({
  systemPrompt: 'You write briefs.',
  subagents: [{
    name: 'researcher',
    description: 'Researches a cause.',
    systemPrompt: 'Edit the note, then report.',
  }],
});

// The lead's model asks for:
task({
  subagent_type: 'researcher',
  description:
    'Late refunds: /notes/late-refund.md',
})
```

- Ses messages reçoivent toute la conversation du chercheur, environ 650 tokens ; ses fichiers ne changent pas.
- Ses messages reçoivent un résultat d'outil, le rapport de 40 tokens ; ses fichiers ne changent pas, parce que le chercheur a modifié une copie.
- **✓** Ses messages reçoivent un résultat d'outil, le rapport de 40 tokens, et la note modifiée est fusionnée dans ses fichiers.
- Rien ne change tant que l'agent principal ne lit pas lui-même /notes/late-refund.md.

**Pourquoi:** task lance le chercheur avec un contexte neuf : son propre system prompt et la description, aucun des messages de l'agent principal. Ce qu'il lit reste dans son propre contexte. Seul son dernier message revient, comme résultat de l'outil task, donc l'agent principal grandit d'environ 40 tokens, et les fichiers que le chercheur a écrits sont fusionnés dans le state.files de l'agent principal. Dans le lab, trois chercheurs ont lu des milliers de tokens pendant que l'agent principal grandissait de trois courts rapports.

## Ce que vous savez faire maintenant

- Activer la liste de tâches d'un deep agent en ajoutant todoListMiddleware() à son middleware, et lire le plan dans state.todos.
- Garder notes et brouillons dans state.files avec ls, read\_file, write\_file, edit\_file, glob et grep.
- Confier un travail autonome à des sous-agents task, et utiliser les rapports et les fichiers qui reviennent.
- Mettre un deep agent en pause avant des outils choisis avec interruptOn et un checkpointer, et retirer l'échafaudage dont un modèle plus récent n'a plus besoin.

---

[Retour à Agents avancés](README.md) · [Suivant : Agents à long horizon](10-long-horizon.md)
