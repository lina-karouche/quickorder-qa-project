# BUG-04 — Le paiement PayPal crée deux commandes identiques

- **Lié à** : TC-M05
- **Contexte** : lors d'un paiement PayPal, une commande est créée, puis une seconde commande strictement identique est créée juste après (web et API).
- **Étapes** :
  1. Checkout avec paiement "paypal"
  2. Confirmer
  3. Consulter `/orders`
- **Résultat attendu** : une seule commande par paiement.
- **Résultat obtenu** : deux commandes identiques apparaissent dans l'historique (double débit potentiel).
- **Preuve** : _[capture d'écran à insérer]_
- **Sévérité / Priorité** : **Bloquant** / P1
