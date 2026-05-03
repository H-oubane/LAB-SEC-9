# Drozer Lab 9 — Walkthrough
## Analyse de sécurité Android avec Drozer

**Application analysée :** Sieve Password Manager  
**Package :** com.withsecure.example.sieve  
**Outil :** Drozer v3.1.0  
**Environnement :** Genymotion Android 11 (API 30)  
**Date :** 03/05/2026  

---

## Outils utilisés

| Outil | Role |
|---|---|
| ADB | Communication avec l'emulateur |
| Drozer v3.1.0 | Analyse de securite Android |
| Drozer Agent APK | Agent installe sur l'emulateur |
| Sieve APK | Application vulnérable de test |
| Genymotion | Emulateur Android |

---

## Etape 1 — Installation de Drozer

### Installation via pip

```bash
pip install drozer
```
---

<img width="1470" height="538" alt="image" src="https://github.com/user-attachments/assets/dfdf2989-c25a-40a9-b23a-e7d181f8c824" />

---

### Verification

```bash
drozer
```

Resultat attendu :
```
usage: drozer [COMMAND]
Commands:
    agent    create custom drozer Agents
    console  start the drozer Console
    ...
```
---

<img width="973" height="432" alt="image" src="https://github.com/user-attachments/assets/b61e0449-b23a-4571-af70-5d943ef2faef" />

---

## Etape 2 — Installation des APK sur l'emulateur

### Installation de l'agent Drozer

```bash
adb install drozer-agent.apk
```
---
<img width="1076" height="132" alt="image" src="https://github.com/user-attachments/assets/d93346b3-ab4b-46c0-a96b-8e99a4e85c11" />

---

### Installation de Sieve

```bash
adb install sieve.apk
```

Resultat attendu pour les deux :
```
Performing Streamed Install
Success
```
---

<img width="963" height="137" alt="image" src="https://github.com/user-attachments/assets/2cea3c89-9d31-4487-b695-1c28cddb83a3" />

---

## Etape 3 — Configuration de Drozer

### Activation du serveur dans l'agent

Sur l'emulateur, ouvrir l'application Drozer Agent et activer "Embedded Server".
---

<img width="506" height="1017" alt="image" src="https://github.com/user-attachments/assets/c18cd211-be23-40c4-8c01-d44190d5be6e" />

---


### Port forwarding

```bash
adb forward tcp:31415 tcp:31415
```
---
<img width="743" height="72" alt="image" src="https://github.com/user-attachments/assets/d0d88a4b-6d51-4939-8e7c-cfd54e709f86" />

---

### Connexion a la console Drozer

```bash
drozer console connect
```

Resultat attendu :
```
drozer Console (v3.1.0)
dz>
```

---
<img width="1025" height="622" alt="image" src="https://github.com/user-attachments/assets/b08c8853-4716-41be-8597-f3dababb7a69" />

---

## Etape 4 — Cartographie des composants

### Liste des applications

```
run app.package.list
```
---

<img width="1242" height="436" alt="image" src="https://github.com/user-attachments/assets/9766ec0e-e490-4b4b-967f-c95029aaeec3" />

---
### Localisation de Sieve

```
run app.package.list -f sieve
```

Resultat :
```
com.withsecure.example.sieve (Sieve)
```
---

<img width="693" height="102" alt="image" src="https://github.com/user-attachments/assets/6a2dd1ae-4118-40f7-822d-1f6cb9479e62" />

---

### Informations sur l'application

```
run app.package.info -a com.withsecure.example.sieve
```

Resultat :
```
Application Label: Sieve
Version: 1.0
Uses Permissions:
- android.permission.INTERNET
- com.withsecure.example.sieve.READ_KEYS
- com.withsecure.example.sieve.WRITE_KEYS
```

---

<img width="1410" height="517" alt="image" src="https://github.com/user-attachments/assets/4c5c88e4-0665-47c5-bab5-055ec63115d6" />

---
### Activities exportees

```
run app.activity.info -a com.withsecure.example.sieve
```

Resultat :
```
Package: com.withsecure.example.sieve
  com.withsecure.example.sieve.activity.MainLoginActivity
    Permission: null
  com.withsecure.example.sieve.activity.FileSelectActivity
    Permission: null
  com.withsecure.example.sieve.activity.PWList
    Permission: null
```

---

<img width="936" height="263" alt="image" src="https://github.com/user-attachments/assets/59df792d-af8f-43c7-be8a-1651ebda37f7" />

---
### Services exportes

```
run app.service.info -a com.withsecure.example.sieve
```

Resultat :
```
Package: com.withsecure.example.sieve
  com.withsecure.example.sieve.service.AuthService
    Permission: null
  com.withsecure.example.sieve.service.CryptoService
    Permission: null
```
---

<img width="890" height="242" alt="image" src="https://github.com/user-attachments/assets/31be347b-5546-4314-b9b6-e06e75c1918e" />

---

### Broadcast Receivers

```
run app.broadcast.info -a com.withsecure.example.sieve
```

Resultat :
```
androidx.profileinstaller.ProfileInstallReceiver
    Permission: android.permission.DUMP
```
---

<img width="771" height="177" alt="image" src="https://github.com/user-attachments/assets/f85cd7cb-5364-4e43-b794-ebc8b74f2461" />

---

### Content Providers

```
run app.provider.info -a com.withsecure.example.sieve
```

Resultat :
```
Authority: com.withsecure.example.sieve.provider.DBContentProvider
    Read Permission: null
    Write Permission: null
    Path Permissions:
      Path: /Keys
        Read Permission: com.withsecure.example.sieve.READ_KEYS
        Write Permission: com.withsecure.example.sieve.WRITE_KEYS
Authority: com.withsecure.example.sieve.provider.FileBackupProvider
    Read Permission: null
    Write Permission: null
```
---

<img width="1257" height="627" alt="image" src="https://github.com/user-attachments/assets/b5bd2b4a-cb94-450a-b7f3-789b9b2818c9" />


---

## Tableau recapitulatif des composants

| Type | Composant | Exporte | Protection |
|---|---|---|---|
| Activity | MainLoginActivity | Oui | Aucune |
| Activity | FileSelectActivity | Oui | Aucune |
| Activity | PWList | Oui | Aucune |
| Service | AuthService | Oui | Aucune |
| Service | CryptoService | Oui | Aucune |
| Provider | DBContentProvider | Oui | Partielle (/Keys uniquement) |
| Provider | FileBackupProvider | Oui | Aucune |
| Receiver | ProfileInstallReceiver | Oui | android.permission.DUMP |

---

## Etape 5 — Verification des protections

### URIs trouvees

```
run app.provider.finduri com.withsecure.example.sieve
```

Resultat :
```
content://com.withsecure.example.sieve.provider.DBContentProvider/Passwords/
content://com.withsecure.example.sieve.provider.DBContentProvider/Keys
content://com.withsecure.example.sieve.provider.DBContentProvider/Passwords
content://com.withsecure.example.sieve.provider.FileBackupProvider
```
---

<img width="1091" height="395" alt="image" src="https://github.com/user-attachments/assets/075e1b2a-fc8a-4653-a696-eaa494a06bd4" />

---

### URIs accessibles sans permission

```
run scanner.provider.finduris -a com.withsecure.example.sieve
```

Resultat :
```
For sure accessible content URIs:
  content://com.withsecure.example.sieve.provider.DBContentProvider/Passwords/
  content://com.withsecure.example.sieve.provider.DBContentProvider/Keys/
  content://com.withsecure.example.sieve.provider.DBContentProvider/Passwords
```
---

<img width="1418" height="502" alt="image" src="https://github.com/user-attachments/assets/c081e3c0-9204-401b-a682-d4b99e2dfcf4" />

---


### Analyse du manifeste

```
run app.package.manifest com.withsecure.example.sieve
```

Points importants trouves dans le manifeste :
- `android:debuggable="true"` — mode debug active en production
- `android:allowBackup="true"` — sauvegarde autorisee
- 3 activities avec `exported="true"` sans permission
- 2 services avec `exported="true"` sans permission
- DBContentProvider avec `exported="true"` et protection partielle
- FileBackupProvider avec `exported="true"` sans aucune protection

---

<img width="1303" height="927" alt="image" src="https://github.com/user-attachments/assets/65383cff-7506-4db3-8243-4ade5d4eb77f" />

---

## Etape 6 — Tests de vulnerabilite

### Test d'injection SQL

```
run scanner.provider.injection -a com.withsecure.example.sieve
```

Resultat :
```
Injection in Projection:
  content://com.withsecure.example.sieve.provider.DBContentProvider/Passwords/
  content://com.withsecure.example.sieve.provider.DBContentProvider/Keys/
  content://com.withsecure.example.sieve.provider.DBContentProvider/Passwords
Injection in Selection:
  content://com.withsecure.example.sieve.provider.DBContentProvider/Passwords/
  content://com.withsecure.example.sieve.provider.DBContentProvider/Keys/
  content://com.withsecure.example.sieve.provider.DBContentProvider/Passwords
```

---

<img width="1142" height="558" alt="image" src="https://github.com/user-attachments/assets/d7ac4d5a-53dd-45e9-b708-ce9f7dff5949" />

---

### Test de path traversal

```
run scanner.provider.traversal -a com.withsecure.example.sieve
```

Resultat :
```
Vulnerable Providers:
  content://com.withsecure.example.sieve.provider.FileBackupProvider/
  content://com.withsecure.example.sieve.provider.FileBackupProvider
```

---

<img width="1077" height="456" alt="image" src="https://github.com/user-attachments/assets/01456f1a-c2d1-4dc7-b508-0c82789149fa" />

---

## Vulnerabilites identifiees

| ID | Composant | Vulnerabilite | Severite |
|---|---|---|---|
| V1 | PWList Activity | Exportee sans protection | Critique |
| V2 | DBContentProvider/Passwords | Injection SQL | Critique |
| V3 | FileBackupProvider | Path Traversal | Critique |
| V4 | DBContentProvider/Keys | URI accessible sans permission | Critique |
| V5 | FileSelectActivity | Exportee sans protection | Elevee |
| V6 | AuthService | Service exporte sans validation | Elevee |
| V7 | CryptoService | Service exporte sans validation | Elevee |
| V8 | Application | android:debuggable=true | Elevee |
| V9 | Application | android:allowBackup=true | Moyenne |
| V10 | READ/WRITE_KEYS | Permission niveau dangerous | Moyenne |

---

## Recommandations

1. Definir `android:exported="false"` pour FileSelectActivity et PWList
2. Ajouter des permissions sur DBContentProvider et FileBackupProvider
3. Definir `android:debuggable="false"` en production
4. Ajouter des permissions sur AuthService et CryptoService
5. Definir `android:allowBackup="false"`
6. Implementer la validation des entrees dans les Content Providers
7. Utiliser `protectionLevel="signature"` pour les permissions sensibles

---

## Mapping OWASP MASVS

| Vulnerabilite | Reference MASVS |
|---|---|
| Activities exportees sans protection | MSTG-PLATFORM-1 |
| Content Providers mal proteges | MSTG-STORAGE-2 |
| Services exportes sans validation | MSTG-PLATFORM-2 |
| Mode debug active | MSTG-RESILIENCE-2 |
| Sauvegarde autorisee | MSTG-STORAGE-8 |



## Auteur
**H-oubane**
