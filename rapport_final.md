# Rapport d'audit de sécurité - Application Android
## Sieve Password Manager

## Informations générales
- **Application** : Sieve Password Manager
- **Package** : com.withsecure.example.sieve
- **Version** : 1.0
- **Date d'audit** : 03/05/2026
- **Auditeur** : Houda
- **Outil utilisé** : Drozer v3.1.0
- **Environnement** : Émulateur Genymotion Android 11 (API 30)

---

## Résumé exécutif

L'analyse de sécurité de l'application **Sieve Password Manager** révèle un niveau de risque **CRITIQUE**. L'application présente de nombreuses vulnérabilités graves liées à des composants Android mal configurés, notamment des activités, services et providers exportés sans protection adéquate. Les vulnérabilités les plus critiques permettent l'accès non autorisé aux mots de passe stockés via injection SQL et path traversal sur les Content Providers exposés.

**Niveau de risque global : CRITIQUE**

---

## Méthodologie

- Analyse statique du manifeste Android via Drozer
- Cartographie des composants exposés avec Drozer
- Vérification des protections en place
- Scan des URIs accessibles sans permission
- Test d'injection SQL sur les Content Providers
- Test de path traversal sur les Content Providers
- Analyse des risques potentiels

---

## Découvertes principales

### 1. Activities exportées sans protection (CRITIQUE)

Trois activités sont exportées sans aucune permission requise :

- `MainLoginActivity` — exportée avec intent-filter MAIN/LAUNCHER
- `FileSelectActivity` — exportée sans protection
- `PWList` — exportée sans protection

Un attaquant peut lancer directement `PWList` pour accéder à la liste des mots de passe sans s'authentifier.

### 2. Services exportés sans protection (ÉLEVÉE)

Deux services sont exportés sans aucune permission :

- `AuthService` — service d'authentification accessible sans protection
- `CryptoService` — service de chiffrement accessible sans protection

### 3. Content Providers vulnérables à l'injection SQL (CRITIQUE)

Les URIs suivantes sont vulnérables à l'injection SQL :

- `content://com.withsecure.example.sieve.provider.DBContentProvider/Passwords/`
- `content://com.withsecure.example.sieve.provider.DBContentProvider/Keys/`
- `content://com.withsecure.example.sieve.provider.DBContentProvider/Passwords`

### 4. Content Provider vulnérable au path traversal (CRITIQUE)

Le FileBackupProvider est vulnérable au path traversal :

- `content://com.withsecure.example.sieve.provider.FileBackupProvider/`
- `content://com.withsecure.example.sieve.provider.FileBackupProvider`

### 5. Mode débogage activé (ÉLEVÉE)

`android:debuggable="true"` est activé dans le manifeste, permettant à un attaquant de connecter un débogueur à l'application.

### 6. Sauvegarde autorisée (MOYENNE)

`android:allowBackup="true"` permet l'extraction des données via ADB sans root.

---

## Recommandations prioritaires

1. **Désactiver l'export des activités sensibles** — Définir `android:exported="false"` pour `FileSelectActivity` et `PWList`
2. **Protéger les Content Providers** — Ajouter des permissions de lecture/écriture sur `DBContentProvider` et `FileBackupProvider`
3. **Désactiver le débogage en production** — Définir `android:debuggable="false"`
4. **Protéger les services** — Ajouter des permissions sur `AuthService` et `CryptoService`
5. **Désactiver la sauvegarde** — Définir `android:allowBackup="false"`

---

## Annexes

### Annexe A — Tableau de triage complet

Voir fichier `triage.csv`

### Annexe B — Mapping OWASP MASVS

| Vulnérabilité | Référence MASVS |
|---|---|
| Activities exportées sans protection | MSTG-PLATFORM-1 |
| Content Providers mal protégés | MSTG-STORAGE-2 |
| Services exportés sans validation | MSTG-PLATFORM-2 |
| Mode débogage activé | MSTG-RESILIENCE-2 |
| Sauvegarde autorisée | MSTG-STORAGE-8 |

### Annexe C — Composants exposés

#### Activities
```
com.withsecure.example.sieve.activity.MainLoginActivity  - Permission: null
com.withsecure.example.sieve.activity.FileSelectActivity - Permission: null
com.withsecure.example.sieve.activity.PWList             - Permission: null
```

#### Services
```
com.withsecure.example.sieve.service.AuthService   - Permission: null
com.withsecure.example.sieve.service.CryptoService - Permission: null
```

#### Content Providers
```
DBContentProvider  - Read: null / Write: null (sauf /Keys)
FileBackupProvider - Read: null / Write: null
```

#### URIs accessibles sans permission
```
content://com.withsecure.example.sieve.provider.DBContentProvider/Passwords/
content://com.withsecure.example.sieve.provider.DBContentProvider/Keys/
content://com.withsecure.example.sieve.provider.DBContentProvider/Passwords
```

#### URIs vulnérables à l'injection SQL
```
content://com.withsecure.example.sieve.provider.DBContentProvider/Passwords/
content://com.withsecure.example.sieve.provider.DBContentProvider/Keys/
content://com.withsecure.example.sieve.provider.DBContentProvider/Passwords
```

#### URIs vulnérables au path traversal
```
content://com.withsecure.example.sieve.provider.FileBackupProvider/
content://com.withsecure.example.sieve.provider.FileBackupProvider
```
