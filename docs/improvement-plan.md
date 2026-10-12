# Plan d'amélioration — QuickOrder V3.1

Améliorations priorisées suite à l'audit qualité. Priorité basée sur l'impact métier et la sévérité des anomalies associées (`04-bug-reports.md`).

| # | Amélioration | Justification | Lié à | Priorité |
|---|---|---|---|---|
| 1 | Revalider le stock au moment du paiement (checkout web et API), pas seulement à l'ajout au panier | Empêche la survente ; risque direct de rupture d'engagement client | BUG-05, BUG-10 | **P1** |
| 2 | Dédupliquer la création de commande PayPal (une seule commande par paiement, idempotence) | Évite un double débit perçu par le client et des stats de vente faussées | BUG-04 | **P1** |
| 3 | Bloquer le checkout si `subtotal < minimum_order` du restaurant | Le minimum affiché doit être une contrainte réelle, pas juste informative | BUG-01 | P2 |
| 4 | Empêcher toute commande sur un restaurant `is_open=False` | Cohérence entre statut affiché et comportement réel | BUG-02 | P2 |
| 5 | Valider que le montant cash reçu ≥ total avant de confirmer un paiement cash | Évite des commandes "payées" avec un montant insuffisant | BUG-03 | P2 |
| 6 | Corriger la valeur du coupon ATLAS15 (10 % → 15 %) ou corriger le texte promotionnel | Incohérence marketing/système = perte de confiance client | BUG-06 | P2 |
| 7 | Corriger le sens du filtre "frais de livraison maximum" sur l'accueil | Le filtre produit actuellement l'inverse de l'attente utilisateur | BUG-07 | P2 |
| 8 | Masquer ou désactiver l'ajout au panier pour les produits à stock = 0 | Clarté pour le client, cohérence avec la logique de stock | BUG-08 | P3 |
| 9 | Restreindre la rédaction d'avis aux utilisateurs ayant une commande livrée sur ce restaurant | Fiabilité/crédibilité du système de notation | BUG-09 | P3 |
| 10 | Revoir les transitions de statut de commande côté admin (interdire les retours en arrière, ex. delivered → confirmed) | Intégrité du cycle de vie des commandes, traçabilité | Analyse code `admin.py` | P3 |
| 11 | Ne pas faire confiance au `subtotal` envoyé par le client sur `/api/coupons/validate` : recalculer côté serveur à partir du panier réel | Manipulation possible de la remise appliquée | Analyse code `api.py` | P2 |
| 12 | Étendre la recherche (web + API) à la description du restaurant, pas seulement au nom | Pertinence des résultats de recherche | Analyse code `main.py` / `api.py` | P4 |

**Synthèse** : 3 améliorations P1 (critiques, à traiter avant mise en production), 6 en P2, 2 en P3, 1 en P4.
