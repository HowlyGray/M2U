# Guide de déploiement sur Vercel

## Prérequis

- Un compte GitHub, GitLab ou Bitbucket
- Un compte Vercel ([vercel.com](https://vercel.com))
- Le code de l'application PeerText

## Méthode 1 : Déploiement via l'interface Vercel

### 1. Préparer le dépôt Git

```bash
# Initialiser git si ce n'est pas déjà fait
git init

# Ajouter tous les fichiers
git add .

# Faire le premier commit
git commit -m "Initial commit - PeerText P2P messaging app"

# Créer un dépôt sur GitHub et pousser
git remote add origin https://github.com/votre-username/peertext.git
git branch -M main
git push -u origin main
```

### 2. Déployer sur Vercel

1. Connectez-vous sur [vercel.com](https://vercel.com)
2. Cliquez sur "Add New..." → "Project"
3. Importez votre dépôt GitHub/GitLab/Bitbucket
4. Configurez le projet :
   - **Framework Preset** : Other
   - **Root Directory** : `./`
   - **Build Command** : (laisser vide)
   - **Output Directory** : `./`
5. Cliquez sur "Deploy"

Vercel déploiera automatiquement votre application et vous fournira une URL HTTPS.

## Méthode 2 : Déploiement via Vercel CLI

### 1. Installer Vercel CLI

```bash
npm install -g vercel
```

### 2. Se connecter

```bash
vercel login
```

### 3. Déployer

```bash
# Dans le répertoire du projet
cd "e:/Web App/M2U"
vercel
```

Suivez les instructions :
- `Link to existing project?` → `No`
- `What's your project's name?` → `peertext`
- `In which directory is your code located?` → `./`
- `Want to modify these settings?` → `No`

### 4. Déployer en production

```bash
vercel --prod
```

## Configuration HTTPS

Vercel fournit automatiquement des certificats HTTPS gratuits pour tous les déploiements. C'est essentiel pour WebRTC qui nécessite un contexte sécurisé.

## Serveurs ICE configurés

L'application inclut déjà les serveurs ICE suivants :

### Serveurs STUN (Publics)
- `stun:stun.l.google.com:19302`
- `stun:stun1.l.google.com:19302`
- `stun:stun2.l.google.com:19302`
- `stun:global.stun.twilio.com:3478`

### Serveurs TURN (OpenRelay - Gratuit pour tests)
- `turn:openrelay.metered.ca:80` (username: `openrelayproject`, credential: `openrelayproject`)
- `turn:openrelay.metered.ca:443` (username: `openrelayproject`, credential: `openrelayproject`)
- `turns:openrelay.metered.ca:443` (username: `openrelayproject`, credential: `openrelayproject`)

Ces serveurs permettent à l'application de fonctionner sur internet, même derrière des NAT/Firewalls.

## Configuration de serveurs TURN personnalisés (Production)

Pour un usage en production avec un serveur TURN dédié :

### Option 1 : Héberger votre propre serveur TURN (coturn)

Sur un VPS Linux :

```bash
# Sur Ubuntu/Debian
sudo apt update
sudo apt install coturn

# Éditer la configuration
sudo nano /etc/turnserver.conf

# Ajouter :
listening-port=3478
fingerprint
lt-cred-mech
user=votre-user:votre-password
realm=votre-domaine.com

# Redémarrer le service
sudo systemctl restart coturn
```

Ensuite, configurez l'interface de PeerText avec votre serveur TURN.

### Option 2 : Services TURN payants

- **Twilio Network Traversal** : fiable, avec API
- **Xirsys** : offres gratuites et payantes
- **Metered TURN** : facile à utiliser

## Configuration via l'interface utilisateur

Une fois déployé, les utilisateurs peuvent personnaliser les serveurs ICE :

1. Cliquez sur le bouton ⚙️ (Paramètres) dans le coin supérieur droit
2. Ajoutez des serveurs STUN/TURN personnalisés
3. La configuration est sauvegardée localement dans le navigateur

## Surveillance et logs

Sur Vercel :
1. Allez sur votre projet
2. Cliquez sur "Deployments"
3. Sélectionnez un déploiement pour voir les logs

## Mises à jour

Pour mettre à jour l'application :

```bash
git add .
git commit -m "Description des changements"
git push
```

Vercel déploiera automatiquement les changements sur la prochaine build.

## Sécurité

- ✅ HTTPS automatique via Vercel
- ✅ Chiffrement E2E AES-256-GCM pour les messages
- ✅ Chiffrement E2E pour les fichiers
- ✅ Pas de stockage de données côté serveur
- ✅ Configuration sécurisée via vercel.json

## Support et dépannage

### Problème de connexion P2P

- Vérifiez que les deux utilisateurs sont sur HTTPS
- Essayez d'ajouter des serveurs TURN supplémentaires
- Vérifiez les firewalls/antivirus locaux

### Serveur TURN inaccessible

- Vérifiez que le serveur TURN est accessible sur internet
- Vérifiez les ports (3478 pour TURN, 5349 pour TURNS)
- Testez avec un outil comme [trickle-ice](https://webrtc.github.io/samples/src/content/peerconnection/trickle-ice/)

### Performance lente

- Utilisez des serveurs TURN géographiquement proches
- Limitez la taille des fichiers transférés
- Vérifiez votre connexion internet
