# Rapport d'anomalies — QuickOrder V3.1

Chaque anomalie ci-dessous a été repérée par analyse du comportement attendu vs. observé pendant l'exécution des cas de test (`02-cas-de-test.md`). **Reproduisez chaque scénario et insérez votre capture d'écran** à l'emplacement indiqué avant de considérer le rapport final.

Sévérité : Bloquant / Majeur / Mineur / Cosmétique — Priorité : P1 (urgent) → P4 (bas)

---

## BUG-01 — Le minimum de commande du restaurant n'est jamais appliqué
- **Lié à** : TC-M03
- **Contexte** : chaque restaurant affiche un `minimum_order` (ex. Couscous Atlas = 25,00 €), mais rien ne bloque une commande en dessous de ce montant.
- **Étapes** : 1) Ouvrir "Couscous Atlas" 2) Ajouter seulement "Thé à la menthe" (3,50 €) au panier 3) Aller au checkout et confirmer
- **Résultat attendu** : la commande devrait être refusée tant que le sous-total est inférieur à 25,00 €.
- **Résultat obtenu** : la commande est acceptée malgré un sous-total très inférieur au minimum affiché.
- **Preuve** : _[capture d'écran à insérer]_
- **Sévérité / Priorité** : Majeur / P2

## BUG-02 — Un restaurant fermé peut quand même recevoir des commandes
- **Lié à** : TC-M02
- **Contexte** : "Night Sushi" est marqué fermé (`is_open=False`, badge "Fermé aujourd'hui") mais ses produits restent commandables.
- **Étapes** : 1) Ouvrir la fiche "Night Sushi" 2) Ajouter "Late Sushi Box" au panier 3) Finaliser la commande
- **Résultat attendu** : impossible de commander un restaurant fermé.
- **Résultat obtenu** : la commande aboutit normalement malgré le statut "fermé".
- **Preuve** : _[capture d'écran à insérer]_
- **Sévérité / Priorité** : Majeur / P2

## BUG-03 — Paiement en espèces accepté même si le montant reçu est insuffisant
- **Lié à** : TC-M04
- **Contexte** : au checkout, le paiement "cash" accepte n'importe quel montant saisi, même inférieur au total dû.
- **Étapes** : 1) Checkout avec paiement "cash" 2) Saisir un montant reçu inférieur au total (ex. 5,00 € pour une commande à 15,00 €) 3) Confirmer
- **Résultat attendu** : la commande devrait être bloquée ou un message d'erreur affiché si le montant reçu < total.
- **Résultat obtenu** : la commande est validée avec un "change_due" négatif, sans aucun blocage.
- **Preuve** : _[capture d'écran à insérer]_
- **Sévérité / Priorité** : Majeur / P2

## BUG-04 — Le paiement PayPal crée deux commandes identiques
- **Lié à** : TC-M05
- **Contexte** : lors d'un paiement PayPal, une commande est créée, puis une seconde commande strictement identique est créée juste après (web et API).
- **Étapes** : 1) Checkout avec paiement "paypal" 2) Confirmer 3) Consulter `/orders`
- **Résultat attendu** : une seule commande par paiement.
- **Résultat obtenu** : deux commandes identiques apparaissent dans l'historique (double débit potentiel).
- **Preuve** : _[capture d'écran à insérer]_
- **Sévérité / Priorité** : **Bloquant** / P1

## BUG-05 — Vente possible malgré un stock insuffisant (survente)
- **Lié à** : TC-L01 / TC-L02
- **Contexte** : le stock n'est jamais revérifié ni au moment de l'ajout au panier, ni au paiement ; il est seulement décrémenté après coup, pouvant devenir négatif.
- **Étapes** : 1) Ouvrir "Tokyo Bowl" 2) Ajouter "Salmon Bowl" (stock=2) avec une quantité de 5 3) Finaliser la commande
- **Résultat attendu** : la quantité devrait être plafonnée au stock disponible (2), ou la commande refusée.
- **Résultat obtenu** : la commande est acceptée avec quantité 5, le stock du produit devient négatif.
- **Preuve** : _[capture d'écran à insérer]_
- **Sévérité / Priorité** : **Bloquant** / P1

## BUG-06 — Le coupon ATLAS15 n'applique pas la remise annoncée
- **Lié à** : TC-N05 (variante)
- **Contexte** : le texte promotionnel de "Couscous Atlas" annonce "Code ATLAS15 annoncé comme -15 %", mais le coupon enregistré applique en réalité 10 %.
- **Étapes** : 1) Ouvrir "Couscous Atlas" (lire le texte promo) 2) Constituer un panier ≥ 20 € 3) Appliquer le code `ATLAS15` au checkout
- **Résultat attendu** : remise de 15 % conforme à la publicité.
- **Résultat obtenu** : remise de 10 % seulement appliquée — incohérence entre le marketing et le système.
- **Preuve** : _[capture d'écran à insérer]_
- **Sévérité / Priorité** : Majeur / P2

## BUG-07 — Le filtre "livraison max" retourne l'inverse de ce qui est demandé
- **Lié à** : TC-N02 (variante)
- **Contexte** : sur l'accueil, filtrer par frais de livraison maximum (ex. 2,50 €) devrait exclure les restaurants plus chers, mais fait l'inverse.
- **Étapes** : 1) Sur l'accueil, régler le filtre "frais de livraison max" à 2,50 € 2) Observer les résultats
- **Résultat attendu** : seuls les restaurants avec frais de livraison ≤ 2,50 € (ex. Green Kitchen à 1,99 €) devraient apparaître.
- **Résultat obtenu** : seuls les restaurants avec frais de livraison **≥** 2,50 € apparaissent (filtre inversé).
- **Preuve** : _[capture d'écran à insérer]_
- **Sévérité / Priorité** : Majeur / P2

## BUG-08 — Les produits en rupture de stock restent affichés comme commandables
- **Lié à** : TC-L01
- **Contexte** : les produits avec `stock=0` (ex. "Veggie Burger", "Gyozas", "Quinoa Bowl") restent visibles sur la fiche restaurant sans aucune indication de rupture, et peuvent être ajoutés au panier.
- **Étapes** : 1) Ouvrir "Burger Factory" 2) Observer "Veggie Burger" 3) L'ajouter au panier
- **Résultat attendu** : le produit devrait être marqué "épuisé"/indisponible et non ajoutable.
- **Résultat obtenu** : aucune indication de rupture, ajout au panier accepté normalement.
- **Preuve** : _[capture d'écran à insérer]_
- **Sévérité / Priorité** : Mineur / P3

## BUG-09 — Un utilisateur peut laisser un avis sans avoir commandé
- **Lié à** : analyse complémentaire (hors cas de test formel)
- **Contexte** : le formulaire d'avis d'un restaurant ne vérifie pas que l'utilisateur connecté a déjà passé commande sur ce restaurant.
- **Étapes** : 1) Se connecter avec "bob" (jamais commandé chez "Pizza Roma") 2) Laisser un avis 5 étoiles sur "Pizza Roma"
- **Résultat attendu** : l'avis devrait être refusé ou nécessiter une commande livrée préalable.
- **Résultat obtenu** : l'avis est publié sans contrôle.
- **Preuve** : _[capture d'écran à insérer]_
- **Sévérité / Priorité** : Mineur / P3

## BUG-10 — L'API `/api/orders` ne revalide pas le stock
- **Lié à** : TC-L02 (variante API)
- **Contexte** : contrairement à la disponibilité (`available`), la quantité en stock n'est jamais vérifiée par l'API avant de créer une commande.
- **Étapes** : `POST /api/orders` avec un `product_id` dont le stock est 0 (ex. "Gyozas") et `quantity: 5`
- **Résultat attendu** : code 409/400, commande refusée.
- **Résultat obtenu** : code 201, commande créée malgré le stock insuffisant.
- **Preuve** : _[capture d'écran / réponse JSON à insérer]_
- **Sévérité / Priorité** : **Bloquant** / P1

---

**Total : 10 bug reports** (minimum requis : 8)
