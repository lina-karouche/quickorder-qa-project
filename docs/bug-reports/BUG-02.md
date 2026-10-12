# BUG-02 — Un restaurant fermé peut quand même recevoir des commandes

- **Lié à** : TC-M02
- **Contexte** : "Night Sushi" est marqué fermé (`is_open=False`, badge "Fermé aujourd'hui") mais ses produits restent commandables.
- **Étapes** :
  1. Ouvrir la fiche "Night Sushi"
  2. Ajouter "Late Sushi Box" au panier
  3. Finaliser la commande
- **Résultat attendu** : impossible de commander un restaurant fermé.
- **Résultat obtenu** : la commande aboutit normalement malgré le statut "fermé".
- **Preuve** : _[capture d'écran à insérer]_
- **Sévérité / Priorité** : Majeur / P2
