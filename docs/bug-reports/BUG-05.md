# BUG-05 — Vente possible malgré un stock insuffisant (survente)

- **Lié à** : TC-L01 / TC-L02
- **Contexte** : le stock n'est jamais revérifié ni au moment de l'ajout au panier, ni au paiement ; il est seulement décrémenté après coup, pouvant devenir négatif.
- **Étapes** :
  1. Ouvrir "Tokyo Bowl"
  2. Ajouter "Salmon Bowl" (stock=2) avec une quantité de 5
  3. Finaliser la commande
- **Résultat attendu** : la quantité devrait être plafonnée au stock disponible (2), ou la commande refusée.
- **Résultat obtenu** : la commande est acceptée avec quantité 5, le stock du produit devient négatif.
- **Preuve** : _[capture d'écran à insérer]_
- **Sévérité / Priorité** : **Bloquant** / P1
