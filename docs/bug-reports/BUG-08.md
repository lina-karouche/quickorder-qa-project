# BUG-08 — Les produits en rupture de stock restent affichés comme commandables

- **Lié à** : TC-L01
- **Contexte** : les produits avec `stock=0` (ex. "Veggie Burger", "Gyozas", "Quinoa Bowl") restent visibles sur la fiche restaurant sans aucune indication de rupture, et peuvent être ajoutés au panier.
- **Étapes** :
  1. Ouvrir "Burger Factory"
  2. Observer "Veggie Burger"
  3. L'ajouter au panier
- **Résultat attendu** : le produit devrait être marqué "épuisé"/indisponible et non ajoutable.
- **Résultat obtenu** : aucune indication de rupture, ajout au panier accepté normalement.
- **Preuve** : _[capture d'écran à insérer]_
- **Sévérité / Priorité** : Mineur / P3
