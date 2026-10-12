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
- Tests utilisateurs / recueil de feedback (partie 6 du brief — non traitée dans cette itération).
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

## 4. Analyse de risques — matrice priorité / impact

| Risque | Zone | Probabilité | Impact | Priorité |
|---|---|---|---|---|
| Survente (commande possible malgré stock insuffisant) | Panier / Checkout / API orders | Élevée | Élevé | **Critique** |
| Paiement cash accepté sans montant suffisant | Checkout / API orders | Élevée | Élevé | **Critique** |
| Commande dupliquée via PayPal (double débit potentiel) | Checkout / API orders | Élevée | Élevé | **Critique** |
| Restaurant fermé acceptant quand même des commandes | Panier / Checkout | Moyenne | Élevé | Haute |
| Minimum de commande affiché mais non appliqué | Checkout | Élevée | Moyen | Haute |
| Incohérence coupon annoncé vs appliqué (ATLAS15) | Coupons | Élevée | Moyen | Haute |
| Filtre de livraison inversé (résultats contraires à la demande) | Recherche / Accueil | Élevée | Moyen | Moyenne |
| Transitions de statut de commande incohérentes (admin) | Back-office | Moyenne | Moyen | Moyenne |
| Avis possible sans avoir commandé | Fiche restaurant | Faible | Faible | Basse |
| API expose des données non censées être publiques (restaurants fermés) | API | Moyenne | Faible | Basse |

## 5. Environnement de test

- App : `http://127.0.0.1:5000` (Flask dev server, SQLite locale, base réinitialisée via `seed.py`)
- Swagger : `http://127.0.0.1:5000/docs/` — clé API `X-API-Key: quickorder-demo-key`
- Comptes : `alice@example.com` / `Password123!` (client), `bob@example.com` / `Password123!` (client), `admin@quickorder.local` / `Admin123!` (admin)
- Outils : navigateur (tests UI), Swagger UI / curl / Postman (tests API)

## 6. Critères d'entrée / sortie

**Entrée** : application démarrée, base de données seedée, comptes de démonstration disponibles.

**Sortie (fin de campagne)** :
- 100 % des cas de test priorité Critique/Haute exécutés
- Tous les bugs trouvés documentés avec preuve
- Rapport d'analyse et recommandation Go/No-Go produits

## 7. Livrables

1. Stratégie de test (ce document)
2. Cas de test (`02-cas-de-test.md`)
3. Preuves d'exécution (`03-campagne-execution.md`)
4. Bug reports (`04-bug-reports.md`)
5. Rapport d'analyse (`05-rapport-analyse.md`)
6. ~~Tests utilisateurs~~ — hors scope de cette itération
7. Plan d'amélioration (`06-plan-amelioration.md`)
