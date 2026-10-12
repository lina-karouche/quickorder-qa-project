# BUG-07 — Le filtre "livraison max" retourne l'inverse de ce qui est demandé

- **Lié à** : TC-N02 (variante)
- **Contexte** : sur l'accueil, filtrer par frais de livraison maximum (ex. 2,50 €) devrait exclure les restaurants plus chers, mais fait l'inverse.
- **Étapes** :
  1. Sur l'accueil, régler le filtre "frais de livraison max" à 2,50 €
  2. Observer les résultats
- **Résultat attendu** : seuls les restaurants avec frais de livraison ≤ 2,50 € (ex. Green Kitchen à 1,99 €) devraient apparaître.
- **Résultat obtenu** : seuls les restaurants avec frais de livraison **≥** 2,50 € apparaissent (filtre inversé).
- **Preuve** : _[capture d'écran à insérer]_
- **Sévérité / Priorité** : Majeur / P2
