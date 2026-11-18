# Agents à long horizon

Agents avancés · Chapitre 10 sur 10 · 25 minutes

Un travail qui s'étend sur plusieurs sessions a besoin de compaction dans une session, de fichiers de passation et de mémoire entre les sessions, et d'une vérification de toute sa trajectoire.

## Objectifs

- Lancez la session 1 et lisez les fichiers qu'elle laisse à la session suivante.
- Démarrez la session 2 à partir des seuls fichiers, et regardez la compaction remplacer les anciens messages par un résumé.
- Notez les appels d'outils des sessions de code, et ouvrez une règle que la session 3 a enfreinte.
- Réussissez la vérification.

## La lecture

### Plus long qu'un contexte

Un travail à long horizon demande plus d'appels au modèle qu'une fenêtre de contexte n'en contient, ou plus d'une session : construire une app fonctionnalité par fonctionnalité, par exemple. La fenêtre de contexte est le maximum qu'un modèle lit en un appel, compté en tokens. Ici, une session est une exécution de l'agent sur un nouveau thread. Deux choses gardent un tel travail sur les rails : réduire le contexte dans une session, et passer le travail d'une session à l'autre.

### Déchargement et compaction

Le billet de LangChain sur la gestion du contexte dans Deep Agents (2026-01-28) décrit trois couches. Un résultat d'outil de plus de 20 000 tokens part dans un fichier, et l'agent reçoit son chemin et un aperçu. Au-delà de 85 % de la fenêtre, les entrées des anciennes écritures et modifications de fichiers deviennent des renvois vers leurs fichiers. Quand cela ne suffit pas, la compaction : un modèle écrit un résumé de l'intention, des artefacts et des prochaines étapes, le résumé remplace les anciens messages, et la transcription complète reste sur le disque. Le harnais recréé de ce lab décharge à partir de 20 000 tokens et compacte à 85 % de la fenêtre (summarize: { window, fraction, keep }), sans couche intermédiaire.

### Fichiers de passation et mémoire

Le billet d'Anthropic sur les harnais pour agents de longue durée (2025-11-26) commence chaque travail par une session d'initialisation. Elle écrit une liste de fonctionnalités en JSON où toutes échouent, un fichier de progression, un script init.sh et un premier commit git. Chaque session suivante lit le fichier de progression, le log git et la liste de fonctionnalités, lance un test de bout en bout de base, travaille sur une fonctionnalité, met son champ passes à true seulement après l'avoir testée, et finit par un commit et une note de progression. Rien d'autre ne change dans la liste. Pour les faits dont chaque thread a besoin, le Store de LangGraph garde une mémoire à long terme sous un namespace, hors de tout thread.

### Réinitialisation ou compaction

Démarrer une session à partir de ces fichiers est une réinitialisation du contexte : un contexte propre et une passation écrite. La compaction garde un seul contexte et le réduit. Dans le billet d'Anthropic du 2026-03-24, Sonnet 4.5 bouclait trop tôt à mesure que son contexte se remplissait, donc ce harnais réinitialisait ; avec Opus 4.5, il est passé à la compaction.

### Noter la trajectoire

Une trajectoire est la suite des appels d'outils qu'une exécution a faits. Une réponse peut dire « terminé » alors qu'aucun test n'a tourné, donc notez les appels avec des règles en code, comme les vérifications en code de Systèmes d'agents, chapitre 8. Lisez les appels dans la trace : après la compaction, les messages ne contiennent plus les premiers.

## Essayez : Trois sessions, un travail

Lancez la session 1, puis la session 2 à partir de ses seuls fichiers, puis la session 3, et notez les trajectoires des sessions de code.

https://agenticademy.org/fr/paths/advanced-agents/10-long-horizon

## Vérification

Une session de code d'un long travail démarre sur un nouveau thread. Mettez ses étapes dans l'ordre, tel que le harnais du billet d'Anthropic les exécute.

1. Lire progress.txt, le log git et feature\_list.json.
2. Lancer le test de fumée pour vérifier que l'app fonctionne encore.
3. Écrire le code d'une fonctionnalité.
4. Lancer les tests de cette fonctionnalité.
5. Mettre passes de cette fonctionnalité à true dans feature\_list.json.
6. Ajouter une ligne à progress.txt et faire un commit.

**Pourquoi:** Le nouveau thread ne sait que ce que disent les fichiers, donc il les lit d'abord. Le test de fumée repère ce que la dernière session a laissé cassé avant que du nouveau code s'ajoute par-dessus. Une fonctionnalité passe seulement quand ses propres tests réussissent sur le nouveau code : mettre passes d'abord, c'est le raccourci que le vérificateur a signalé dans la session 3. La ligne de progression et le commit sont ce que lit la session suivante.

## Ce que vous savez faire maintenant

- Préparer un long travail avec une liste de fonctionnalités, un fichier de progression et des commits, pour que chaque session puisse partir des fichiers.
- Compacter un contexte avec summarize à un seuil, et décharger les gros résultats d'outils dans des fichiers.
- Garder une mémoire à long terme dans un Store de LangGraph que chaque thread lit.
- Noter une exécution par ses appels d'outils, avec des règles en code lues dans la trace.

---

[Retour à Agents avancés](README.md)
