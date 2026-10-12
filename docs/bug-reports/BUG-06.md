# BUG-06 — Le coupon ATLAS15 n'applique pas la remise annoncée

- **Lié à** : TC-N05 (variante)
- **Contexte** : le texte promotionnel de "Couscous Atlas" annonce "Code ATLAS15 annoncé comme -15 %", mais le coupon enregistré applique en réalité 10 %.
- **Étapes** :
  1. Ouvrir "Couscous Atlas" (lire le texte promo)
  2. Constituer un panier ≥ 20 €
  3. Appliquer le code `ATLAS15` au checkout
- **Résultat attendu** : remise de 15 % conforme à la publicité.
- **Résultat obtenu** : remise de 10 % seulement appliquée — incohérence entre le marketing et le système.
- **Preuve** : _[capture d'écran à insérer]_
- **Sévérité / Priorité** : Majeur / P2
