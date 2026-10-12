# BUG-09 — Un utilisateur peut laisser un avis sans avoir commandé

- **Lié à** : analyse complémentaire (hors cas de test formel)
- **Contexte** : le formulaire d'avis d'un restaurant ne vérifie pas que l'utilisateur connecté a déjà passé commande sur ce restaurant.
- **Étapes** :
  1. Se connecter avec "bob" (jamais commandé chez "Pizza Roma")
  2. Laisser un avis 5 étoiles sur "Pizza Roma"
- **Résultat attendu** : l'avis devrait être refusé ou nécessiter une commande livrée préalable.
- **Résultat obtenu** : l'avis est publié sans contrôle.
- **Preuve** : _[capture d'écran à insérer]_
- **Sévérité / Priorité** : Mineur / P3
