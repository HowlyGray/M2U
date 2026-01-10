# 🔥 Firebase - Découverte Automatique des Peers

## ⚡ TL;DR - Démarrage Rapide

```bash
1. Créer projet Firebase → https://console.firebase.google.com/
2. Activer Realtime Database (mode test)
3. Copier la config Firebase
4. Coller dans index.html (ligne 3344)
5. Changer FIREBASE_ENABLED = true (ligne 3342)
6. Déployer et c'est tout ! 🎉
```

## 📖 Documentation complète

Voir **FIREBASE_SETUP.md** pour le guide pas-à-pas avec captures d'écran.

## 🎯 Que fait Firebase ?

Firebase Realtime Database agit comme un **annuaire en temps réel** où tous les utilisateurs s'enregistrent automatiquement :

```
Firebase Database
    /peers
        /PT-ABC123 → {name: "Kenny", online: true, timestamp: ...}
        /PT-DEF456 → {name: "Alice", online: true, timestamp: ...}
        /PT-GHI789 → {name: "Bob", online: true, timestamp: ...}
```

### Sans Firebase (actuel)
```
Utilisateur A démarre
  ↓
  ❌ Aucun utilisateur visible
  ↓
Doit se connecter MANUELLEMENT à B
  ↓
Ensuite découverte automatique fonctionne
```

### Avec Firebase (après configuration)
```
Utilisateur A démarre
  ↓
  ✅ S'enregistre automatiquement dans Firebase
  ↓
Utilisateur B démarre
  ↓
  ✅ S'enregistre et voit A instantanément
  ↓
  ✅ Connexion P2P automatique établie
  ↓
  ✅ Aucune intervention manuelle nécessaire !
```

## ✨ Fonctionnalités

### Auto-registration
- Chaque utilisateur s'enregistre automatiquement au démarrage
- Données stockées : `{name, timestamp, online, peerId}`
- Auto-cleanup à la déconnexion (Firebase onDisconnect)

### Real-time Discovery
- Écoute en temps réel des nouveaux peers (`child_added`)
- Mise à jour des changements de nom (`child_changed`)
- Détection des déconnexions (`child_removed`)

### Auto-connect
- Connexion P2P automatique dès qu'un peer est découvert
- Pas besoin d'échanger manuellement les IDs
- Bootstrap peers sauvegardés pour futures sessions

### Heartbeat
- Mise à jour timestamp toutes les 30s
- Prouve que l'utilisateur est actif
- Permet de détecter les connexions fantômes

## 📊 Données stockées

Par peer dans Firebase :
```json
{
  "name": "Kenny",           // Nom d'utilisateur
  "timestamp": 1234567890,   // Timestamp serveur
  "online": true,            // Statut en ligne
  "peerId": "PT-ABC123"      // ID du peer
}
```

**Taille par peer** : ~100 bytes
**100 utilisateurs** : ~10KB
**Largement dans les quotas gratuits !**

## 🔒 Sécurité

### Mode Test (initial)
```json
{
  "rules": {
    ".read": true,
    ".write": true
  }
}
```
⚠️ Tout le monde peut lire/écrire (OK pour développement)

### Mode Production (recommandé)
```json
{
  "rules": {
    "peers": {
      "$peerId": {
        ".read": true,
        ".write": "$peerId === newData.child('peerId').val()"
      }
    }
  }
}
```
✅ Lecture publique, écriture uniquement sur son propre peer

## 💰 Coûts (Plan Gratuit)

Firebase Spark (gratuit) inclut :
- ✅ 1 GB stockage
- ✅ 10 GB bande passante/mois
- ✅ 100 connexions simultanées

**Pour PeerText** :
- 100 utilisateurs actifs = ~10KB stockage
- Heartbeat toutes les 30s = ~100 KB/jour/utilisateur
- **Total pour 100 utilisateurs : ~3 GB/mois**

**Verdict** : ✅ Gratuit pour plusieurs centaines d'utilisateurs !

## 🚀 Alternatives si pas Firebase

### Option 1 : Vercel KV
Créer `/api/peers.js` qui gère un cache Redis.

### Option 2 : Supabase Realtime
Alternative open-source à Firebase.

### Option 3 : PeerJS Cloud Server
Utiliser `/peerjs/peers` API (si disponible sur votre serveur).

### Option 4 : Système actuel (Bootstrap)
Connexion manuelle initiale puis découverte automatique.

## 📱 Compatibilité

- ✅ Chrome/Edge/Brave
- ✅ Firefox
- ✅ Safari (iOS/macOS)
- ✅ Tous navigateurs modernes avec WebRTC
- ⚠️ Nécessite HTTPS (pas `file://`)

## 🔍 Debug

### Console logs à chercher
```
✅ Firebase initialisé - Découverte automatique activée!
🔥 Firebase: Enregistré avec succès
🔥 Firebase: Écoute des peers activée
🔥 Firebase: Peer découvert: Kenny (PT-ABC123)
🔗 Firebase: Connexion automatique à PT-ABC123
```

### Erreurs courantes

**"Firebase SDK non chargé"**
→ CDN Firebase bloqué ? Vérifiez réseau

**"Permission denied"**
→ Règles Firebase incorrectes (voir FIREBASE_SETUP.md)

**"FIREBASE_ENABLED is not defined"**
→ Vérifiez que le code Firebase est bien dans index.html

**"databaseURL is required"**
→ Ajoutez `databaseURL` dans firebaseConfig

## 📈 Monitoring

Firebase Console → Realtime Database → Usage :
- Nombre de connexions actives
- Bande passante utilisée
- Nombre d'opérations

Firebase Console → Realtime Database → Données :
- Voir tous les peers en ligne en temps réel
- Vérifier les timestamps
- Debug les problèmes de connexion

## 🎓 Ressources

- [Firebase Docs](https://firebase.google.com/docs/database)
- [Firebase Console](https://console.firebase.google.com/)
- [Firebase Pricing](https://firebase.google.com/pricing)
- [Guide complet : FIREBASE_SETUP.md](./FIREBASE_SETUP.md)

---

**Une fois configuré, plus JAMAIS besoin de connexion manuelle ! 🎉**
