# BUG-01 — Le minimum de commande du restaurant n'est jamais appliqué

- **Lié à** : TC-M03
- **Contexte** : chaque restaurant affiche un `minimum_order` (ex. Couscous Atlas = 25,00 €), mais rien ne bloque une commande en dessous de ce montant.
- **Étapes** :
  1. Ouvrir "Couscous Atlas"
  2. Ajouter seulement "Thé à la menthe" (3,50 €) au panier
  3. Aller au checkout et confirmer
- **Résultat attendu** : la commande devrait être refusée tant que le sous-total est inférieur à 25,00 €.
- **Résultat obtenu** : la commande est acceptée malgré un sous-total très inférieur au minimum affiché.
- **Preuve** : _[capture d'écran à insérer]_
- **Sévérité / Priorité** : Majeur / P2
