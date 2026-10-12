# BUG-10 — L'API `/api/orders` ne revalide pas le stock

- **Lié à** : TC-L02 (variante API)
- **Contexte** : contrairement à la disponibilité (`available`), la quantité en stock n'est jamais vérifiée par l'API avant de créer une commande.
- **Étapes** : `POST /api/orders` avec un `product_id` dont le stock est 0 (ex. "Gyozas") et `quantity: 5`
- **Résultat attendu** : code 409/400, commande refusée.
- **Résultat obtenu** : code 201, commande créée malgré le stock insuffisant.
- **Preuve** : _[capture d'écran / réponse JSON à insérer]_
- **Sévérité / Priorité** : **Bloquant** / P1
