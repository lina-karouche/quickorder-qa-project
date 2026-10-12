# Matrice de risques — QuickOrder V3.1

Analyse de risques priorisée par probabilité × impact, utilisée pour prioriser l'ordre d'exécution des cas de test (`test-cases.xlsx`).

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
| API fait confiance au `subtotal` envoyé par le client pour le calcul de remise | API coupons | Moyenne | Moyen | Haute |

## Méthode de priorisation

- **Critique** : probabilité élevée + impact élevé → à tester en premier, bloquant pour le Go-live si confirmé.
- **Haute** : probabilité élevée/impact moyen ou l'inverse → à tester avant la fin de la campagne.
- **Moyenne** : impact limité au confort utilisateur, pas de perte financière directe.
- **Basse** : edge cases, impact cosmétique ou périmètre restreint.
