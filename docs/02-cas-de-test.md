# Cas de test — QuickOrder V3.1

Légende statut : ✅ Pass · ❌ Fail · ⬜ À exécuter · 🚫 Blocked

Pour chaque cas : **Résultat obtenu** et **Statut** sont à compléter pendant la campagne d'exécution (`03-campagne-execution.md`), capture d'écran à l'appui.

---

## A. Cas nominaux (6)

### TC-N01 — Connexion avec identifiants valides
- **Objectif** : vérifier qu'un client peut se connecter avec des identifiants corrects.
- **Préconditions** : compte `alice@example.com` existant.
- **Données** : email `alice@example.com`, mot de passe `Password123!`
- **Étapes** : 1) Aller sur `/login` 2) Saisir email/mot de passe 3) Cliquer "Se connecter"
- **Résultat attendu** : redirection vers l'accueil, utilisateur authentifié (nom affiché).
- **Résultat obtenu** : _à compléter_
- **Statut** : ⬜

### TC-N02 — Recherche de restaurant par nom
- **Objectif** : vérifier que la recherche retourne les restaurants correspondants.
- **Préconditions** : aucune.
- **Données** : recherche `Tokyo`
- **Étapes** : 1) Sur l'accueil, saisir "Tokyo" dans la barre de recherche 2) Valider
- **Résultat attendu** : "Tokyo Bowl" apparaît dans les résultats.
- **Résultat obtenu** : _à compléter_
- **Statut** : ⬜

### TC-N03 — Ajout d'un produit disponible au panier
- **Objectif** : vérifier l'ajout standard d'un produit au panier.
- **Préconditions** : connecté ou non (le panier fonctionne en session).
- **Données** : "Classic Burger" (Burger Factory), quantité 1
- **Étapes** : 1) Ouvrir la fiche "Burger Factory" 2) Ajouter "Classic Burger" au panier
- **Résultat attendu** : message de confirmation, panier contient 1 "Classic Burger", sous-total = 11,90 €.
- **Résultat obtenu** : _à compléter_
- **Statut** : ⬜

### TC-N04 — Passage de commande complet (paiement carte)
- **Objectif** : vérifier le parcours de checkout nominal de bout en bout.
- **Préconditions** : connecté (alice), panier avec au moins 1 produit disponible.
- **Données** : "Classic Burger" x1, adresse par défaut, paiement "card"
- **Étapes** : 1) Ajouter au panier 2) Aller au checkout 3) Choisir adresse + paiement carte 4) Confirmer
- **Résultat attendu** : commande créée, statut "confirmed", redirection vers le détail de commande, panier vidé.
- **Résultat obtenu** : _à compléter_
- **Statut** : ⬜

### TC-N05 — Application d'un coupon valide (WELCOME10)
- **Objectif** : vérifier le calcul de remise avec un coupon valide.
- **Préconditions** : panier avec sous-total ≥ 20 €.
- **Données** : coupon `WELCOME10` (10 %, min 20 €, plafond 8 €), sous-total = 23,80 € (2x Classic Burger)
- **Étapes** : 1) Au checkout, saisir le code `WELCOME10` 2) Valider
- **Résultat attendu** : remise appliquée = 2,38 € (10 % de 23,80 €, sous le plafond de 8 €).
- **Résultat obtenu** : _à compléter_
- **Statut** : ⬜

### TC-N06 — Annulation d'une commande "confirmed"
- **Objectif** : vérifier qu'une commande au statut annulable peut être annulée par son propriétaire.
- **Préconditions** : commande existante au statut "confirmed".
- **Données** : commande créée en TC-N04
- **Étapes** : 1) Aller sur `/orders/<id>` 2) Cliquer "Annuler"
- **Résultat attendu** : statut passe à "cancelled".
- **Résultat obtenu** : _à compléter_
- **Statut** : ⬜

---

## B. Cas aux limites (6)

### TC-L01 — Produit en rupture de stock (stock = 0)
- **Objectif** : vérifier le comportement quand le stock est à 0.
- **Préconditions** : aucune.
- **Données** : "Veggie Burger" (Burger Factory, stock=0) ou "Gyozas" (Tokyo Bowl, stock=0)
- **Étapes** : 1) Ouvrir la fiche restaurant 2) Observer le produit à stock 0 3) Tenter de l'ajouter au panier
- **Résultat attendu** : le produit devrait être visible comme indisponible/non commandable (épuisé).
- **Résultat obtenu** : _à compléter_
- **Statut** : ⬜

### TC-L02 — Stock quasi épuisé (stock = 2)
- **Objectif** : vérifier la limite haute de quantité commandable.
- **Préconditions** : aucune.
- **Données** : "Salmon Bowl" (Tokyo Bowl, stock=2), quantité demandée = 3
- **Étapes** : 1) Ajouter "Salmon Bowl" au panier avec quantité 3
- **Résultat attendu** : la quantité devrait être plafonnée à 2 (stock disponible), ou un message d'erreur devrait bloquer l'ajout.
- **Résultat obtenu** : _à compléter_
- **Statut** : ⬜

### TC-L03 — Coupon au montant minimum exact
- **Objectif** : vérifier le comportement à la borne exacte du montant minimum.
- **Préconditions** : panier avec sous-total = exactement 10,00 €.
- **Données** : coupon `LUNCH5` (5 %, min 10 €), sous-total = 10,00 €
- **Étapes** : 1) Constituer un panier à 10,00 € exactement 2) Appliquer `LUNCH5`
- **Résultat attendu** : remise appliquée (10,00 € n'est pas < 10,00 €, donc coupon valide).
- **Résultat obtenu** : _à compléter_
- **Statut** : ⬜

### TC-L04 — Coupon juste sous le montant minimum
- **Objectif** : vérifier le rejet juste en dessous de la borne.
- **Préconditions** : panier avec sous-total = 9,99 €.
- **Données** : coupon `LUNCH5` (min 10 €), sous-total = 9,99 €
- **Étapes** : 1) Constituer un panier à 9,99 € 2) Appliquer `LUNCH5`
- **Résultat attendu** : message "Montant minimum de 10,00 € requis", remise = 0.
- **Résultat obtenu** : _à compléter_
- **Statut** : ⬜

### TC-L05 — Coupon avec remise à 100 % plafonnée (TOKYO30)
- **Objectif** : vérifier que le plafond de remise (`max_discount`) est bien appliqué même avec un pourcentage extrême.
- **Préconditions** : panier Tokyo Bowl, sous-total ≥ 30 €.
- **Données** : coupon `TOKYO30` (100 %, min 30 €, plafond 3,49 €)
- **Étapes** : 1) Constituer un panier Tokyo Bowl ≥ 30 € 2) Appliquer `TOKYO30`
- **Résultat attendu** : remise plafonnée à 3,49 €, **pas** 100 % du sous-total.
- **Résultat obtenu** : _à compléter_
- **Statut** : ⬜

### TC-L06 — Commande à la quantité API maximale (20)
- **Objectif** : vérifier la borne haute de quantité acceptée par l'API.
- **Préconditions** : clé API valide.
- **Données** : `POST /api/orders` avec `quantity: 20` puis `quantity: 21`
- **Étapes** : 1) Envoyer une commande avec quantité 20 2) Envoyer la même avec quantité 21
- **Résultat attendu** : 20 accepté (201), 21 rejeté (400 "quantity must be between 1 and 20").
- **Résultat obtenu** : _à compléter_
- **Statut** : ⬜

---

## C. Cas d'erreur / robustesse (6)

### TC-E01 — Connexion avec mot de passe incorrect
- **Objectif** : vérifier le rejet d'identifiants invalides.
- **Données** : `alice@example.com` / `mauvaismdp`
- **Étapes** : 1) Saisir identifiants incorrects 2) Valider
- **Résultat attendu** : message "Email ou mot de passe incorrect", pas de connexion.
- **Résultat obtenu** : _à compléter_
- **Statut** : ⬜

### TC-E02 — Inscription avec mot de passe trop court
- **Objectif** : vérifier la validation du formulaire d'inscription.
- **Données** : nom "Test", email "test@test.com", mot de passe "123"
- **Étapes** : 1) Remplir le formulaire avec mot de passe < 8 caractères 2) Valider
- **Résultat attendu** : message "mot de passe d'au moins 8 caractères requis", compte non créé.
- **Résultat obtenu** : _à compléter_
- **Statut** : ⬜

### TC-E03 — Inscription avec email déjà utilisé
- **Objectif** : vérifier l'unicité de l'email.
- **Données** : email `alice@example.com` (déjà existant)
- **Étapes** : 1) Tenter de s'inscrire avec un email déjà utilisé
- **Résultat attendu** : message "Un compte existe déjà avec cet email".
- **Résultat obtenu** : _à compléter_
- **Statut** : ⬜

### TC-E04 — Appel API sans clé `X-API-Key`
- **Objectif** : vérifier la protection des endpoints API.
- **Données** : `GET /api/restaurants` sans en-tête `X-API-Key`
- **Étapes** : 1) Appeler l'endpoint sans la clé API
- **Résultat attendu** : code HTTP 401, message "missing or invalid X-API-Key".
- **Résultat obtenu** : _à compléter_
- **Statut** : ⬜

### TC-E05 — Création de commande API avec un produit inexistant
- **Objectif** : vérifier la gestion d'ID produit invalide.
- **Données** : `POST /api/orders` avec `product_id: 99999`
- **Étapes** : 1) Envoyer une commande avec un ID produit inexistant
- **Résultat attendu** : code HTTP 400, message "one or more products do not exist".
- **Résultat obtenu** : _à compléter_
- **Statut** : ⬜

### TC-E06 — Validation de coupon avec code inexistant
- **Objectif** : vérifier le comportement avec un coupon invalide.
- **Données** : `POST /api/coupons/validate` avec `code: "FAUXCODE"`
- **Étapes** : 1) Appeler l'endpoint avec un code coupon inexistant
- **Résultat attendu** : `{"valid": false, "error": "Coupon invalide ou expiré."}`, code HTTP 404.
- **Résultat obtenu** : _à compléter_
- **Statut** : ⬜

---

## D. Cas de règles métier (6)

### TC-M01 — Panier mono-restaurant
- **Objectif** : vérifier qu'on ne peut pas mélanger des produits de deux restaurants différents.
- **Données** : "Classic Burger" (Burger Factory) puis "Chicken Teriyaki" (Tokyo Bowl)
- **Étapes** : 1) Ajouter "Classic Burger" au panier 2) Tenter d'ajouter "Chicken Teriyaki"
- **Résultat attendu** : message "Un panier ne peut contenir que des produits du même restaurant", 2e produit refusé.
- **Résultat obtenu** : _à compléter_
- **Statut** : ⬜

### TC-M02 — Commande sur un restaurant fermé (Night Sushi)
- **Objectif** : vérifier qu'un restaurant fermé ne peut pas recevoir de commande.
- **Préconditions** : restaurant "Night Sushi" (`is_open=False`).
- **Données** : "Late Sushi Box" (Night Sushi)
- **Étapes** : 1) Ouvrir la fiche "Night Sushi" 2) Ajouter un produit au panier 3) Aller jusqu'au paiement
- **Résultat attendu** : le système devrait empêcher la commande (restaurant fermé).
- **Résultat obtenu** : _à compléter_
- **Statut** : ⬜

### TC-M03 — Minimum de commande non atteint
- **Objectif** : vérifier que le `minimum_order` du restaurant est appliqué au checkout.
- **Préconditions** : restaurant avec `minimum_order` élevé (ex. Couscous Atlas = 25,00 €).
- **Données** : panier Couscous Atlas < 25,00 € (ex. "Thé à la menthe" seul, 3,50 €)
- **Étapes** : 1) Constituer un panier sous le minimum 2) Tenter de finaliser la commande
- **Résultat attendu** : la commande devrait être bloquée tant que le minimum n'est pas atteint.
- **Résultat obtenu** : _à compléter_
- **Statut** : ⬜

### TC-M04 — Paiement en espèces avec montant insuffisant
- **Objectif** : vérifier le contrôle du montant reçu en cash.
- **Données** : total commande = 15,00 €, `cash_received` = 5,00 €
- **Étapes** : 1) Checkout avec paiement "cash" 2) Saisir un montant reçu inférieur au total 3) Confirmer
- **Résultat attendu** : la commande devrait être refusée ou le montant insuffisant signalé.
- **Résultat obtenu** : _à compléter_
- **Statut** : ⬜

### TC-M05 — Paiement PayPal : vérifier l'unicité de la commande
- **Objectif** : vérifier qu'un seul enregistrement de commande est créé par paiement.
- **Données** : panier quelconque, paiement "paypal"
- **Étapes** : 1) Checkout avec paiement PayPal 2) Confirmer 3) Consulter `/orders`
- **Résultat attendu** : une seule commande créée pour ce paiement.
- **Résultat obtenu** : _à compléter_
- **Statut** : ⬜

### TC-M06 — Accès à la commande d'un autre utilisateur (IDOR)
- **Objectif** : vérifier le contrôle d'accès horizontal sur les commandes.
- **Préconditions** : commande #1 appartenant à alice ; connecté en tant que bob.
- **Données** : `GET /orders/1` (ou `GET /api/orders/1` avec clé API) en étant connecté/bob
- **Étapes** : 1) Se connecter avec bob 2) Tenter d'accéder à l'URL d'une commande d'alice
- **Résultat attendu** : accès refusé (redirection + message, ou 403 côté API).
- **Résultat obtenu** : _à compléter_
- **Statut** : ⬜

---

**Total : 24 cas de test** (6 nominaux + 6 limites + 6 erreurs + 6 règles métier)
