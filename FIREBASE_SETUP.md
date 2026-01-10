# 🔥 Configuration Firebase pour PeerText

## Étape 1 : Créer un projet Firebase (5 minutes)

### 1.1 Créer le projet
1. Allez sur [Firebase Console](https://console.firebase.google.com/)
2. Cliquez sur **"Ajouter un projet"**
3. Nom du projet : `peertext-discovery` (ou votre choix)
4. Désactivez Google Analytics (optionnel pour ce projet)
5. Cliquez sur **"Créer un projet"**

### 1.2 Créer une application Web
1. Dans la console Firebase, cliquez sur l'icône **Web** `</>`
2. Nom de l'application : `PeerText Web`
3. ✅ **NE PAS** cocher "Firebase Hosting"
4. Cliquez sur **"Enregistrer l'application"**

### 1.3 Copier la configuration
Vous verrez un bloc comme ceci :

```javascript
const firebaseConfig = {
  apiKey: "AIzaSyXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX",
  authDomain: "peertext-discovery.firebaseapp.com",
  databaseURL: "https://peertext-discovery-default-rtdb.firebaseio.com",
  projectId: "peertext-discovery",
  storageBucket: "peertext-discovery.appspot.com",
  messagingSenderId: "123456789012",
  appId: "1:123456789012:web:abcdef1234567890"
};
```

**⚠️ GARDEZ CETTE CONFIG POUR L'ÉTAPE 3**

## Étape 2 : Activer Realtime Database

### 2.1 Créer la base de données
1. Dans le menu latéral, cliquez sur **"Realtime Database"**
2. Cliquez sur **"Créer une base de données"**
3. Choisissez la région la plus proche de vous :
   - Europe : `europe-west1`
   - USA : `us-central1`
   - Asie : `asia-southeast1`
4. Mode de sécurité : Choisissez **"Mode test"** (pour commencer)

### 2.2 Configurer les règles de sécurité

Une fois la base créée, allez dans l'onglet **"Règles"** et remplacez par :

```json
{
  "rules": {
    "peers": {
      "$peerId": {
        ".read": true,
        ".write": true,
        ".validate": "newData.hasChildren(['name', 'timestamp', 'online'])",
        ".indexOn": ["timestamp", "online"]
      }
    }
  }
}
```

**Explication des règles** :
- ✅ Lecture publique (tous peuvent voir les peers en ligne)
- ✅ Écriture publique (chacun peut s'enregistrer)
- ✅ Validation des champs requis
- ✅ Index pour performances

Cliquez sur **"Publier"**

## Étape 3 : Configurer PeerText

### 3.1 Ouvrir le fichier de configuration

Dans votre projet, ouvrez `index.html` et cherchez cette section :

```javascript
// ============================================
// FIREBASE CONFIGURATION
// ============================================
```

### 3.2 Remplacer la configuration

Remplacez le bloc `firebaseConfig` par **VOTRE configuration** copiée à l'étape 1.3 :

```javascript
const firebaseConfig = {
  apiKey: "VOTRE_API_KEY",              // ← Remplacez
  authDomain: "VOTRE_PROJECT.firebaseapp.com",
  databaseURL: "https://VOTRE_PROJECT.firebaseio.com",  // ← Important !
  projectId: "VOTRE_PROJECT",
  storageBucket: "VOTRE_PROJECT.appspot.com",
  messagingSenderId: "VOTRE_SENDER_ID",
  appId: "VOTRE_APP_ID"
};
```

### 3.3 Activer Firebase

Changez cette ligne :

```javascript
const FIREBASE_ENABLED = false;  // ← Changez en true
```

en :

```javascript
const FIREBASE_ENABLED = true;   // ✅ Activé !
```

## Étape 4 : Tester

### 4.1 Déployer sur Vercel
Firebase ne fonctionne que via **HTTPS** (pas en `file://`).

```bash
# Si pas encore fait
npm install -g vercel

# Déployer
vercel
```

### 4.2 Ouvrir plusieurs onglets

1. Ouvrez l'URL Vercel dans **2 navigateurs différents** (ou mode incognito)
2. Changez le pseudo dans chaque onglet
3. ✅ **Vérifiez** : Les utilisateurs devraient apparaître automatiquement dans "Utilisateurs en ligne" **SANS connexion manuelle !**

### 4.3 Vérifier Firebase Console

Allez dans Firebase Console → Realtime Database → Onglet **"Données"**

Vous devriez voir :

```
peers/
  ├── PT-ABC12345
  │   ├── name: "Kenny"
  │   ├── online: true
  │   └── timestamp: 1234567890123
  └── PT-DEF67890
      ├── name: "Alice"
      ├── online: true
      └── timestamp: 1234567891234
```

## Étape 5 : Sécurité production (optionnel)

Pour la production, améliorez les règles :

```json
{
  "rules": {
    "peers": {
      "$peerId": {
        ".read": true,
        ".write": "$peerId === newData.child('peerId').val() || !data.exists()",
        ".validate": "newData.hasChildren(['name', 'timestamp', 'online', 'peerId'])",
        ".indexOn": ["timestamp", "online"]
      }
    },
    ".read": false,
    ".write": false
  }
}
```

Cette règle empêche qu'un utilisateur modifie l'entrée d'un autre.

## Quotas Firebase (Plan gratuit)

- ✅ **Connexions simultanées** : 100
- ✅ **Données stockées** : 1 GB
- ✅ **Téléchargement** : 10 GB/mois
- ✅ **Opérations** : 50,000 lectures/jour

**Pour PeerText** : Largement suffisant pour 100+ utilisateurs actifs !

## Dépannage

### Erreur "Permission denied"
→ Vérifiez les règles Firebase (Étape 2.2)

### Erreur "databaseURL is required"
→ Vérifiez que `databaseURL` est bien dans votre config

### Aucun utilisateur ne s'affiche
→ Ouvrez la console (F12) et cherchez les logs `🔥 Firebase:`
→ Vérifiez que `FIREBASE_ENABLED = true`

### "Service Worker failed"
→ Normal en local, fonctionne uniquement en HTTPS (Vercel/serveur)

## Support

Si vous avez des problèmes :
1. Vérifiez les logs console (F12)
2. Vérifiez Firebase Console → Realtime Database → Données
3. Vérifiez les règles de sécurité

---

**Une fois configuré, la découverte sera 100% automatique ! 🎉**
