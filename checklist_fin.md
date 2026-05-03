# Checklist de fin d'audit
## Application : Sieve Password Manager
## Date : 03/05/2026

---

## Conformité de l'audit

- [x] Toutes les étapes du lab ont été suivies
- [x] Tous les composants Android ont été analysés
- [x] Le tableau de triage est complet
- [x] Les remédiations proposées sont spécifiques et applicables
- [x] Le mapping OWASP MASVS est correct

---

## Étapes réalisées

- [x] Étape 1 — Configuration de l'environnement (Drozer + Sieve installés)
- [x] Étape 2 — Connexion et validation du canal Drozer
- [x] Étape 3 — Cartographie des composants Android exposés
- [x] Étape 4 — Vérification des protections (URIs, permissions)
- [x] Étape 5 — Analyse des risques (injection SQL, path traversal)
- [x] Étape 6 — Collecte de preuves

---

## Commandes Drozer exécutées

- [x] `run app.package.list`
- [x] `run app.package.list -f sieve`
- [x] `run app.package.info -a com.withsecure.example.sieve`
- [x] `run app.activity.info -a com.withsecure.example.sieve`
- [x] `run app.activity.info -a com.withsecure.example.sieve -i`
- [x] `run app.service.info -a com.withsecure.example.sieve`
- [x] `run app.broadcast.info -a com.withsecure.example.sieve`
- [x] `run app.provider.info -a com.withsecure.example.sieve`
- [x] `run app.provider.finduri com.withsecure.example.sieve`
- [x] `run scanner.provider.finduris -a com.withsecure.example.sieve`
- [x] `run app.package.manifest com.withsecure.example.sieve`
- [x] `run scanner.provider.injection -a com.withsecure.example.sieve`
- [x] `run scanner.provider.traversal -a com.withsecure.example.sieve`

---

## Absence de données sensibles

- [x] Aucune donnée utilisateur réelle n'est présente dans le rapport
- [x] Aucun mot de passe ou clé n'est inclus dans le rapport
- [x] Les captures d'écran ne contiennent pas d'informations sensibles
- [x] Les chemins système complets ont été anonymisés
- [x] Les identifiants personnels ont été supprimés

---

## Qualité du rapport

- [x] Le rapport est bien structuré
- [x] Les vulnérabilités sont clairement expliquées
- [x] Les recommandations sont précises et actionnables
- [x] La documentation est complète
- [x] Le format des livrables est conforme aux attentes

---

## Vulnérabilités identifiées — Résumé

| Sévérité | Nombre |
|---|---|
| 🔴 Critique | 4 |
| 🟠 Élevée | 4 |
| 🟡 Moyenne | 2 |
| **Total** | **10** |

---

## Score estimé : 18/20

| Critère | Points obtenus | Points max |
|---|---|---|
| Traçabilité | 4 | 4 |
| Cartographie | 4 | 4 |
| Triage | 3 | 3 |
| Mapping OWASP | 3 | 3 |
| Remédiations | 3 | 4 |
| Qualité du rapport | 1 | 2 |
| **Total** | **18** | **20** |
