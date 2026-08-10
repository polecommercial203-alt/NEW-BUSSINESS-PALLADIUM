# Palladium Africa CRM

CRM collaboratif avec authentification Firebase, Firestore, Storage et synchronisation temps réel.

## Principes de production

- Firebase/Firestore = source de vérité.
- Synchronisation temps réel silencieuse.
- Aucun `location.reload()` pour synchroniser les données.
- Les formulaires en cours de saisie ne doivent pas être écrasés par une mise à jour distante.
- Les données métier doivent être protégées par les règles Firestore/Storage, pas seulement par le filtrage de l'interface.
- Ne jamais committer de clé privée, compte de service Firebase Admin ou fichier `.env` réel.

## Installation

1. Cloner le dépôt.
2. Installer les dépendances si le projet en possède.
3. Créer la configuration Firebase à partir de `.env.example` si l'application utilise des variables d'environnement.
4. Configurer le projet Firebase.
5. Déployer les règles Firestore et Storage après revue.
6. Tester avec au moins deux comptes utilisateurs avant production.

## Déploiement

Avant tout déploiement production :

- vérifier les règles Firestore ;
- vérifier les règles Storage ;
- vérifier les index Firestore ;
- tester création/modification simultanées ;
- tester hors connexion/reconnexion ;
- tester plusieurs onglets ;
- vérifier les permissions de chaque rôle ;
- vérifier qu'aucune donnée existante n'est perdue.

## Sécurité

Les informations suivantes ne doivent jamais être poussées dans GitHub :

- Firebase Admin SDK service account ;
- clés privées ;
- certificats privés ;
- mots de passe ;
- tokens ;
- secrets API serveur ;
- fichiers `.env` de production.

La configuration Firebase frontend n'est pas équivalente à une clé privée, mais les règles Firebase doivent rester strictes.

## Documentation

Voir `AUDIT_PRODUCTION.md` et `AUDIT_FOND_PALLADIUM_CLAUDE.md` pour les points de contrôle et critères de validation.
