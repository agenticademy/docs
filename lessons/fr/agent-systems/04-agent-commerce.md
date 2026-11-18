# Des agents qui paient : UCP et AP2

Systèmes d'agents et standards · Chapitre 4 sur 8 · 25 minutes

Comment un agent passe commande chez un marchand avec UCP, et comment les mandats AP2 lient le paiement à ce que la personne a signé.

## Objectifs

- Créez et terminez une session de paiement.
- Signez les mandats sur le téléphone.
- Modifiez le montant après la signature et voyez la vérification échouer.
- Réussissez la vérification.

## La lecture

### UCP : le paiement

UCP (Universal Commerce Protocol, version 2026-08-25) est la façon dont un agent d'achat, la plateforme, achète chez un marchand, l'entreprise. L'entreprise publie un profil à /.well-known/ucp : sa version, ses capacités comme checkout, ses gestionnaires de paiement et son endpoint REST. L'agent le lit, puis crée une session de paiement avec POST /checkout-sessions, la met à jour avec PUT jusqu'à ce que le statut soit ready\_for\_complete, et termine avec POST …/complete.

Chaque appel porte un en-tête UCP-Agent qui pointe vers le profil de la plateforme, pour que l'entreprise puisse vérifier ce que la plateforme prend en charge. Chaque écriture porte une Idempotency-Key : si un délai dépassé pousse l'agent à renvoyer la même requête, l'entreprise répond avec le premier résultat au lieu d'agir deux fois. Un double prélèvement comme celui de la commande 48213 de Halden est exactement ce qu'elle empêche.

### AP2 : la signature de la personne

UCP seul fait confiance à quiconque détient la session. AP2 (Agent Payments Protocol) ajoute une preuve de consentement. Le fournisseur de l'agent émet deux mandats sous forme de verifiable credentials SD-JWT : un mandat de commande qui contient un hash de la commande exacte que Halden a signée, et un mandat de paiement qui contient le montant, le bénéficiaire et la carte. La personne les examine sur un écran où l'agent ne peut pas dessiner, le portefeuille de son téléphone, et la clé du téléphone signe un JWT de key binding sur chacun.

Halden compare le mandat de commande à la commande qu'il a signée. Le portefeuille de cartes vérifie le mandat de paiement avant d'émettre un token de paiement pour ce montant, et le processeur vérifie le token avant de débiter. Modifiez le panier ou le montant après la signature de la personne, et un hash ou une signature ne correspond plus, quoi que dise l'agent.

Ici, la personne est présente. Dans le mode autonome d'AP2, elle ne l'est pas : elle approuve à l'avance des mandats ouverts, avec des limites comme un prix maximum, et l'agent signe les mandats finaux dans ces limites.

### ANP, en deux phrases

ANP (Agent Network Protocol) sert aux agents qui se rencontrent sur le web ouvert : chaque agent a un identifiant did:wba dont le document DID, servi depuis son propre domaine, liste ses clés publiques. Chaque requête est signée avec HTTP Message Signatures, donc l'autre partie sait quel agent l'a envoyée sans compte. Les labs ANP de l'étape Aller plus loin le font tourner.

### Ce que vous faites ici

Un agent d'achat achète la tente Ridge 2 de Halden. Suivez le paiement, approuvez sur le téléphone, puis relancez avec Intercepter activé et augmentez le montant après la signature.

## Essayez : Une chaîne de signatures

Cinq étapes, puis deux facultatives. Sous les étapes, la chaîne ajoute chaque maillon de l'achat quand l'achat l'atteint, avec qui le vérifie. Sous la chaîne, le lab dessine chaque message comme une ligne, de haut en bas.

https://agenticademy.org/fr/paths/agent-systems/04-agent-commerce

## Vérification

### Question 1 sur 4

Que lie un mandat de commande AP2 ?

- Le nom de l'agent.
- **✓** Un hash de la commande exacte, avec son panier et son total, que la personne a approuvée.
- Le logo du marchand.

**Pourquoi:** Le mandat est la preuve signée de ce que la personne a accepté d'acheter. Une commande différente a un hash différent, donc elle ne correspond plus.

### Question 2 sur 4

L'agent augmente le montant après la signature de la personne. Où cela échoue-t-il ?

- Nulle part, si l'agent explique pourquoi.
- **✓** À la vérification : le portefeuille de cartes, Halden ou le processeur constate qu'une signature ou un hash ne correspond plus.
- Seulement à la banque, des jours plus tard.

**Pourquoi:** Chaque partie vérifie les signatures par rapport aux valeurs qu'elle a sous les yeux. L'agent ne peut pas négocier avec un hash.

### Question 3 sur 4

Qu'empêche l'Idempotency-Key d'UCP ?

- L'injection de prompt.
- **✓** Qu'une requête renvoyée crée une seconde commande ou un second débit.
- Les cartes expirées.

**Pourquoi:** Une nouvelle tentative réseau répète une requête. Avec la même clé, l'entreprise renvoie le premier résultat au lieu d'agir deux fois : le double prélèvement de l'histoire de Halden est exactement ce qu'elle empêche.

### Question 4 sur 4

Pourquoi la personne approuve-t-elle dans le portefeuille de son téléphone plutôt que dans le chat de l'agent ?

- Les téléphones ont de plus grands écrans.
- **✓** L'agent ne peut pas dessiner sur l'écran du portefeuille, et seul le téléphone détient la clé qui signe les mandats.
- UCP exige un téléphone.

**Pourquoi:** AP2 appelle cela une surface de confiance. La personne voit les mandats tels qu'ils ont été émis, et la signature vient d'une clé que l'agent ne détient jamais.

## Ce que vous savez faire maintenant

- Suivre une session de paiement UCP du profil jusqu'à completed.
- Expliquer comment les mandats AP2 lient un paiement au consentement signé d'une personne.
- Utiliser des clés d'idempotence pour rendre les nouvelles tentatives sûres.
- Dire en deux phrases ce qu'apportent le did:wba et les requêtes signées d'ANP.

---

[Retour à Systèmes d'agents et standards](README.md) · [Suivant : Orchestration : superviseurs, fan-out, sous-graphes](05-orchestration.md)
