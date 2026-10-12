# Stratégie de test — QuickOrder V3.1

**Équipe** : Lina Karouche
**Date** : 2026-10-12
**Version testée** : QuickOrder V3.1 (environnement local, SQLite, seed par défaut)

## 1. Contexte

QuickOrder est une plateforme de commande de repas (web + API REST) qui doit être auditée avant un passage en production. L'objectif n'est pas de corriger le code mais d'évaluer sa qualité : identifier les anomalies, les documenter, et formuler une recommandation Go/No-Go.

## 2. Périmètre

### Inclus
- Parcours client web : inscription, connexion, recherche/filtres de restaurants, consultation produits, panier, checkout, suivi de commande, annulation, recommande, favoris, avis, profil (adresses).
- API REST : `/api/restaurants`, `/api/restaurants/{id}`, `/api/restaurants/{id}/products`, `/api/products/{id}`, `/api/coupons/validate`, `/api/orders` (création/liste/détail/annulation), `/api/favorites`.
- Règles métier : panier mono-restaurant, coupons (montant minimum, plafond, expiration), paiement (carte/PayPal/cash), gestion du stock, statuts de commande, droits admin.

### Exclu (hors périmètre de cet audit)
- Tests utilisateurs / recueil de feedback (partie 6 du brief — voir `user-feedback.md` pour la justification).
- Modification du code source (contrainte explicite du brief : audit en boîte noire/grise sans corriger l'app).
- Tests de charge/performance et tests de sécurité approfondis (hors scope temps imparti) — seuls des contrôles de base (IDOR, clé API) sont inclus.

## 3. Types de tests appliqués

| Type | Objectif |
|---|---|
| Fonctionnel nominal | Vérifier que les parcours standards fonctionnent comme attendu |
| Limites | Vérifier le comportement aux bornes (stock=0, quantité max, montant minimum coupon, expiration) |
| Erreurs / robustesse | Vérifier la gestion des entrées invalides (champs vides, formats incorrects, auth manquante) |
| Règles métier | Vérifier les règles spécifiques au domaine (mono-restaurant, calcul remise, transitions de statut, droits d'accès) |
| API | Vérifier les contrats d'API (codes HTTP, payloads, authentification par clé) |
| Sécurité de base | Contrôle d'accès horizontal (IDOR sur commandes d'autrui), autorisation API |

L'analyse de risques détaillée et la priorisation sont consignées séparément dans `risk-matrix.md`.

## 4. Environnement de test

- App : `http://127.0.0.1:5000` (Flask dev server, SQLite locale, base réinitialisée via `seed.py`)
- Swagger : `http://127.0.0.1:5000/docs/` — clé API `X-API-Key: quickorder-demo-key`
- Comptes : `alice@example.com` / `Password123!` (client), `bob@example.com` / `Password123!` (client), `admin@quickorder.local` / `Admin123!` (admin)
- Outils : navigateur (tests UI), Swagger UI / curl / Postman (tests API)

## 5. Critères d'entrée / sortie

**Entrée** : application démarrée, base de données seedée, comptes de démonstration disponibles.

**Sortie (fin de campagne)** :
- 100 % des cas de test priorité Critique/Haute exécutés
- Tous les bugs trouvés documentés avec preuve
- Rapport d'analyse et recommandation Go/No-Go produits

## 6. Livrables

1. Stratégie de test (ce document) — `test-strategy.md`
2. Matrice de risques — `risk-matrix.md`
3. Cas de test — `test-cases.xlsx`
4. Rapport d'exécution (preuves + statistiques + Go/No-Go) — `execution-report.md`
5. Bug reports — `bug-reports/`
6. Tests utilisateurs — `user-feedback.md`
7. Plan d'amélioration — `improvement-plan.md`
