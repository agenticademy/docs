# Ingénierie du contexte et récupération

Systèmes d'agents et standards · Chapitre 6 sur 8 · 25 minutes

Choisir ce que lit chaque appel au modèle : grep ou recherche vectorielle, déchargement des gros résultats, un premier aperçu des sous-agents, et une mémoire qui dure d'un thread à l'autre.

## Objectifs

- Lancez grep et la recherche vectorielle sur les mêmes mots et comparez.
- Laissez un gros résultat d'outil être déchargé au lieu de remplir le contexte.
- Confiez une recherche à des sous-agents et comparez la taille des contextes.
- Réussissez la vérification.

## La lecture

### Ce que lit chaque appel

Un appel au modèle lit son contexte : le prompt système, les messages jusqu'ici et chaque résultat d'outil. L'ingénierie du contexte, c'est décider de ce qui y entre. Trop peu, et l'agent devine ; trop, et cela coûte plus cher, va plus lentement et le distrait avec du texte sans importance, ou placé là exprès.

### grep ou vecteurs

grep trouve du texte exact, ligne par ligne : un numéro de commande, un id de ticket, une date. Il ne coûte rien et rate tout ce qui est formulé autrement.

La recherche vectorielle trouve un sens proche. Chaque morceau de chaque fichier est transformé en vecteur (ici, 384 nombres issus du modèle d'embedding all-MiniLM-L6-v2), et les morceaux sont classés par similarité cosinus avec le vecteur de la requête. Elle trouve « payment taken two times » pour « charged twice », mais pour 48213 elle classe des tickets sur les commandes 48144 et 48231 devant le bon. Des filtres comme un dossier ou une date écartent les morceaux périmés ou sans rapport.

### Déchargement, skills et sous-agents

Un outil peut renvoyer bien plus qu'un contexte ne peut contenir. L'éviction enregistre un gros résultat dans un fichier et donne au modèle le chemin et un court aperçu ; l'agent fait ensuite un grep sur le fichier et ne lit que les lignes dont il a besoin.

Les skills suivent la même idée : l'agent voit le nom de chaque skill et une description d'une ligne, et ne lit ses instructions complètes (un fichier SKILL.md) que lorsqu'une tâche en a besoin.

Un sous-agent reçoit un contexte neuf avec seulement sa tâche et renvoie un seul rapport, donc ses lectures restent hors du contexte principal. Agents avancés, chapitres 9 et 10, traite en détail les sous-agents, les seuils et la compaction.

### La mémoire d'un thread à l'autre

Un thread est une conversation : son checkpointer garde les messages de ce thread, et un nouveau thread démarre vide. La mémoire à long terme est un store que tous les threads partagent. L'InMemoryStore de LangGraph range des éléments sous un namespace, comme customers / mara-lind ; un thread y dépose une note, et un thread suivant la cherche. En production, le store est une base de données.

### Dans ce lab

L'agent de support de Halden traite le ticket T-48213. Son harnais recrée Deep Agents (un harnais open source de LangChain) sur LangGraph.js : outils de système de fichiers, éviction au-delà de 20 000 tokens et sous-agents de tâche. L'étape de mémoire utilise l'InMemoryStore de LangGraph.js lui-même.

## Essayez : Ce que lit l'agent

Cherchez les mêmes mots avec grep et avec des vecteurs, laissez l'export des paiements être déchargé, confiez la lecture à des sous-agents, puis enregistrez une note qu'un thread suivant relit.

https://agenticademy.org/fr/paths/agent-systems/06-context-engineering

## Vérification

### Question 1 sur 4

L'agent doit trouver chaque ticket qui mentionne la commande 48213. Quelle recherche convient le mieux ?

- La recherche vectorielle.
- **✓** grep sur 48213.
- Demander au modèle de s'en souvenir.

**Pourquoi:** Un id exact, c'est précisément le rôle de grep. La recherche vectorielle classe par sens, et dans ce lab elle classe des tickets sur les commandes 48144 et 48231 devant le bon.

### Question 2 sur 4

Un outil renvoie un export de 40 000 lignes. Que met l'éviction dans le contexte ?

- Les 40 000 lignes.
- **✓** Le chemin du fichier et un court aperçu ; l'agent en lit des pages quand il en a besoin.
- Rien du tout.

**Pourquoi:** Le résultat complet est enregistré dans un fichier, donc le contexte reste petit et l'agent peut toujours faire un grep et parcourir les données page par page.

### Question 3 sur 4

Que renvoie un sous-agent à l'agent qui l'a lancé ?

- Toute sa conversation.
- **✓** Un seul rapport final de son travail.
- Ses outils.

**Pourquoi:** Les lectures du sous-agent restent dans son propre contexte. Seul son rapport rejoint le contexte du parent, ce qui garde le parent petit et le texte placé hors de lui.

### Question 4 sur 4

Le thread 1 a enregistré une note dans le store. Le thread 2 démarre sans messages. Peut-il lire la note ?

- Non : un nouveau thread démarre vide.
- **✓** Oui : tous les threads partagent le store ; seuls les messages appartiennent à un thread.
- Seulement si le thread 2 copie les messages du thread 1.

**Pourquoi:** Le checkpointer garde les messages de chaque thread séparés. Le store est en dehors des threads, donc n'importe quel thread peut chercher dans le namespace de la cliente.

## Ce que vous savez faire maintenant

- Choisir entre grep et la recherche vectorielle pour une requête, et dire quand un filtre par dossier ou par date aide.
- Décharger les gros résultats d'outils et les parcourir page par page.
- Utiliser des sous-agents pour garder le contexte d'un agent principal petit et propre.
- Enregistrer une note dans un store à long terme et la relire dans un thread suivant.

---

[Retour à Systèmes d'agents et standards](README.md) · [Suivant : Observabilité : traces et exécutions](07-observability.md)
