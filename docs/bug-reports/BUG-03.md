# BUG-03 — Paiement en espèces accepté même si le montant reçu est insuffisant

- **Lié à** : TC-M04
- **Contexte** : au checkout, le paiement "cash" accepte n'importe quel montant saisi, même inférieur au total dû.
- **Étapes** :
  1. Checkout avec paiement "cash"
  2. Saisir un montant reçu inférieur au total (ex. 5,00 € pour une commande à 15,00 €)
  3. Confirmer
- **Résultat attendu** : la commande devrait être bloquée ou un message d'erreur affiché si le montant reçu < total.
- **Résultat obtenu** : la commande est validée avec un "change_due" négatif, sans aucun blocage.
- **Preuve** : _[capture d'écran à insérer]_
- **Sévérité / Priorité** : Majeur / P2
