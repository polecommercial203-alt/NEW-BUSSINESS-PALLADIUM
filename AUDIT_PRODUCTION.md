# Palladium CRM — audit et durcissement effectué

## Ce qui a été corrigé dans cette version

### Synchronisation
- suppression du rechargement automatique de page comme mécanisme de synchronisation ;
- synchronisation silencieuse via Firestore `onSnapshot` ;
- aucune notification de synchronisation ;
- conservation d'un formulaire actif pendant une mise à jour distante ;
- fusion à trois états côté application ;
- transactions Firestore pour les écritures ;
- fusion des tableaux d'objets par `id` ;
- fusion champ par champ lorsqu'un même objet est modifié en parallèle ;
- conservation des écritures en attente après échec ;
- retry silencieux avec backoff ;
- flush sur `visibilitychange` / `pagehide` ;
- démarrage hors ligne depuis le cache local si Firestore est momentanément indisponible.

### Réunions
- synchronisation Firebase des réunions ;
- une réunion = un document Firestore ;
- écoute temps réel ;
- cache local conservé pour la compatibilité avec l'interface existante.

### Sécurité
- vérification `active` pour Storage ;
- traçabilité des écritures de tranches ;
- validation de l'auteur des entrées d'audit ;
- règles dédiées aux réunions ;
- les écritures Firestore opérationnelles doivent porter `_by = uid`.

### PWA
- nouvelle version du Service Worker ;
- conservation de Firestore hors du cache HTTP ;
- stratégie réseau-first conservée.

## Tests statiques réalisés

- `pa-firebase.js` : syntaxe JavaScript valide avec Node.js ;
- script applicatif extrait de `index.html` : syntaxe JavaScript valide ;
- `brief.js` : syntaxe JavaScript valide ;
- contrôle des occurrences de `location.reload()` : les occurrences restantes sont liées à la connexion, déconnexion, changement de mot de passe ou écran d'erreur — aucune n'est utilisée par la synchronisation distante.

## Limites qui nécessitent encore une validation Firebase réelle

Ce projet n'est pas connecté depuis cet environnement à la console Firebase de production. Il est donc impossible ici de :
- déployer les règles ;
- vérifier les données actuellement présentes dans Firestore ;
- exécuter un test réel avec deux comptes Firebase ;
- mesurer les quotas / `429 Too Many Requests` ;
- tester les performances avec 10, 25, 50 ou 100 utilisateurs réels.

### Point d'architecture restant

Le CRM historique utilise encore des « tranches » Firestore pour conserver la compatibilité avec ses modules synchrones. La synchronisation est maintenant transactionnelle et beaucoup plus résistante aux conflits, mais une future migration vers un document Firestore par entité métier reste recommandée pour une très forte volumétrie.

**Ne pas prétendre qu'un test multi-utilisateurs réel est réussi avant de l'avoir exécuté sur le projet Firebase de production.**
