# Solution de découverte automatique avec Firebase

## Problème actuel
Le système P2P pur ne peut pas découvrir automatiquement les peers sans point de rencontre initial.

## Solution : Firebase Realtime Database (Gratuit)

### Pourquoi Firebase ?
- ✅ Gratuit (jusqu'à 10GB de transfert/mois)
- ✅ Temps réel (WebSocket)
- ✅ Pas besoin de backend
- ✅ Configuration en 5 minutes
- ✅ Auto-cleanup des peers déconnectés

### Architecture

```
Firebase Realtime DB
    /peers
        /PT-ABC123
            name: "Kenny"
            timestamp: 1234567890
            online: true
        /PT-DEF456
            name: "Anonyme"
            timestamp: 1234567891
            online: true
```

### Flux

1. Utilisateur démarre → Se connecte à Firebase
2. Écrit son peer ID dans `/peers/{myPeerId}`
3. Écoute les changements sur `/peers`
4. Découvre automatiquement tous les autres peers
5. Se connecte en P2P aux peers découverts
6. Au départ : Firebase supprime automatiquement l'entrée

### Setup requis

1. Créer projet Firebase (gratuit)
2. Activer Realtime Database
3. Copier la config Firebase
4. Intégrer le SDK Firebase dans index.html

### Code à ajouter

```javascript
// Firebase config
const firebaseConfig = {
  apiKey: "VOTRE_API_KEY",
  authDomain: "VOTRE_PROJECT.firebaseapp.com",
  databaseURL: "https://VOTRE_PROJECT.firebaseio.com",
  projectId: "VOTRE_PROJECT"
};

// Initialiser Firebase
firebase.initializeApp(firebaseConfig);
const db = firebase.database();

// S'enregistrer au démarrage
function registerInFirebase() {
  const myRef = db.ref(`peers/${state.myPeerId}`);

  myRef.set({
    name: state.myName,
    timestamp: Date.now(),
    online: true
  });

  // Auto-cleanup à la déconnexion
  myRef.onDisconnect().remove();

  // Heartbeat toutes les 30s
  setInterval(() => {
    myRef.update({ timestamp: Date.now() });
  }, 30000);
}

// Écouter les autres peers
function listenForPeers() {
  db.ref('peers').on('child_added', (snapshot) => {
    const peerId = snapshot.key;
    const data = snapshot.val();

    if (peerId !== state.myPeerId) {
      console.log(`🔥 Firebase: Discovered ${data.name} (${peerId})`);
      // Ajouter à discovered peers et connecter
      discoverPeerFromFirebase(peerId, data.name);
    }
  });

  db.ref('peers').on('child_removed', (snapshot) => {
    const peerId = snapshot.key;
    console.log(`🔥 Firebase: ${peerId} disconnected`);
    // Marquer comme offline
  });
}
```

### Avantages
✅ Découverte instantanée et automatique
✅ Pas de connexion manuelle nécessaire
✅ Gratuit et scalable
✅ Auto-cleanup
✅ Temps réel

### Alternative : Vercel KV + API Routes

Si vous préférez rester sur Vercel uniquement :

Créer `/api/peers.js` :
```javascript
import { kv } from '@vercel/kv';

export default async function handler(req, res) {
  if (req.method === 'POST') {
    // Enregistrer peer
    const { peerId, name } = req.body;
    await kv.setex(`peer:${peerId}`, 60, JSON.stringify({ name, timestamp: Date.now() }));
    return res.json({ success: true });
  }

  if (req.method === 'GET') {
    // Lister peers
    const keys = await kv.keys('peer:*');
    const peers = await Promise.all(
      keys.map(async (key) => {
        const data = await kv.get(key);
        return { peerId: key.replace('peer:', ''), ...JSON.parse(data) };
      })
    );
    return res.json(peers);
  }
}
```

Puis dans index.html :
```javascript
// Enregistrement
setInterval(() => {
  fetch('/api/peers', {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({ peerId: state.myPeerId, name: state.myName })
  });
}, 30000);

// Découverte
setInterval(async () => {
  const res = await fetch('/api/peers');
  const peers = await res.json();
  peers.forEach(peer => discoverPeer(peer.peerId, peer.name));
}, 10000);
```

## Recommandation

**Utilisez Firebase** si vous voulez :
- Solution la plus simple et rapide
- Temps réel natif
- Pas de backend à maintenir

**Utilisez Vercel KV** si vous voulez :
- Tout héberger sur Vercel
- Plus de contrôle
- Éviter dépendance externe

Les deux solutions nécessitent une configuration externe mais résolvent le problème de découverte automatique une fois pour toutes.
