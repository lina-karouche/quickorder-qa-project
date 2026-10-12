# Rapport d'exécution — QuickOrder V3.1

> ⚠️ Les preuves (captures d'écran) et les statuts finaux sont **à compléter** au fur et à mesure de l'exécution des 24 cas de test (`test-cases.xlsx`). La recommandation du §3 se base sur les anomalies déjà identifiées par analyse ; ajustez-la si vos résultats réels diffèrent.

## 1. Suivi des preuves d'exécution

Pour chaque cas de test de `test-cases.xlsx`, consigner ici la preuve (capture d'écran, extrait de réponse API, log) puis reporter le statut dans la colonne "Statut" du fichier Excel.

**Comment faire une capture utile** : cadrez la donnée qui prouve le résultat (message affiché, montant calculé, code HTTP + corps JSON pour l'API, ligne de statut de commande...). Nommez vos fichiers `TC-XXX.png` et placez-les dans `docs/preuves/`.

| Cas de test | Date | Exécuté par | Fichier(s) de preuve | Statut |
|---|---|---|---|---|
| TC-N01 | | | | ⬜ |
| TC-N02 | | | | ⬜ |
| TC-N03 | | | | ⬜ |
| TC-N04 | | | | ⬜ |
| TC-N05 | | | | ⬜ |
| TC-N06 | | | | ⬜ |
| TC-L01 | | | | ⬜ |
| TC-L02 | | | | ⬜ |
| TC-L03 | | | | ⬜ |
| TC-L04 | | | | ⬜ |
| TC-L05 | | | | ⬜ |
| TC-L06 | | | | ⬜ |
| TC-E01 | | | | ⬜ |
| TC-E02 | | | | ⬜ |
| TC-E03 | | | | ⬜ |
| TC-E04 | | | | ⬜ |
| TC-E05 | | | | ⬜ |
| TC-E06 | | | | ⬜ |
| TC-M01 | | | | ⬜ |
| TC-M02 | | | | ⬜ |
| TC-M03 | | | | ⬜ |
| TC-M04 | | | | ⬜ |
| TC-M05 | | | | ⬜ |
| TC-M06 | | | | ⬜ |

## 2. Statistiques

| Statut | Nombre | % |
|---|---|---|
| ✅ Pass | _à compléter_ | |
| ❌ Fail | _à compléter_ | |
| 🚫 Blocked | _à compléter_ | |
| ⬜ Skipped | _à compléter_ | |
| **Total** | 24 | 100 % |

### Synthèse des anomalies (voir `bug-reports/`)

| Sévérité | Nombre |
|---|---|
| Bloquant | 3 |
| Majeur | 5 |
| Mineur | 2 |

## 3. Risques résiduels

- **Survente** (BUG-05, BUG-10) : le stock n'est jamais revérifié avant paiement, ni côté web ni côté API. Risque direct d'engagement client non honoré.
- **Double commande PayPal** (BUG-04) : impact financier et confusion pour le client (historique de commande dupliqué).
- **Paiement cash non contrôlé** (BUG-03) : une commande peut être validée "payée" sans que le montant reçu couvre le total.
- **Incohérences d'affichage** (minimum de commande, restaurant fermé, coupon ATLAS15, filtre de livraison) : nuisent à la confiance utilisateur sans bloquer techniquement le service.

## 4. Recommandation Go / No-Go

**No-Go** pour une mise en production en l'état.

**Justification** : 3 anomalies bloquantes touchent directement le cœur du parcours transactionnel (survente, double paiement PayPal, paiement cash non contrôlé). Ces défauts peuvent entraîner une perte financière directe et une dégradation de la confiance client dès les premières commandes réelles.

**Condition de passage en Go** : correction et re-test des 3 anomalies bloquantes au minimum (BUG-04, BUG-05, BUG-10), idéalement également des anomalies majeures avant lancement commercial.

## 5. Prochaines étapes

1. Exécuter les 24 cas de test et compléter les statistiques ci-dessus.
2. Transmettre `bug-reports/` à l'équipe de développement.
3. Prioriser les corrections selon `improvement-plan.md`.
4. Planifier une campagne de non-régression après corrections.
