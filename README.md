# M2U (PeerText)

Une interface gratuite de discussion peer-to-peer chiffrée de bout en bout entre utilisateurs dans le monde entier.

## 🚀 Fonctionnalités

- ✅ **Chiffrement E2E AES-256-GCM** : Messages et fichiers sécurisés
- ✅ **WebRTC P2P** : Communication directe sans serveur intermédiaire
- ✅ **Transfert de fichiers** : Partage de fichiers jusqu'à 100 MB
- ✅ **Multi-utilisateurs** : Support des salons de discussion
- ✅ **Indicateur de frappe** : Voyez quand quelqu'un écrit
- ✅ **Interface moderne** : Design responsive et intuitif
- ✅ **Configuration ICE** : Serveurs STUN/TURN personnalisables
- ✅ **Prêt pour internet** : Fonctionne sur différents réseaux via TURN servers

## 🌐 Déploiement sur Vercel

### Méthode rapide (CLI)

```bash
# Installer Vercel CLI
npm install -g vercel

# Se connecter
vercel login

# Déployer
vercel
```

### Méthode via l'interface web

1. Poussez votre code sur GitHub/GitLab/Bitbucket
2. Importez le projet sur [vercel.com](https://vercel.com)
3. Déployez en un clic

Pour des instructions détaillées, voir [DEPLOYMENT.md](DEPLOYMENT.md)

## 🔧 Configuration des serveurs ICE

L'application inclut déjà des serveurs STUN/TURN gratuits (OpenRelay) pour fonctionner sur internet.

Pour personnaliser les serveurs :

1. Cliquez sur le bouton ⚙️ (Paramètres) dans le coin supérieur droit
2. Ajoutez des serveurs STUN/TURN personnalisés
3. La configuration est sauvegardée localement

### Serveurs TURN pour production

Pour un usage en production, configurez votre propre serveur TURN :

- **Auto-hébergé** : Utilisez [coturn](https://github.com/coturn/coturn) sur un VPS
- **Services cloud** : Twilio, Xirsys, Metered TURN

## 📦 Fichiers de configuration

- `vercel.json` : Configuration Vercel
- `.env.example` : Exemple de variables d'environnement
- `.gitignore` : Fichiers ignorés par Git

## 🔒 Sécurité

- Chiffrement de bout en bout pour tous les messages
- Chiffrement de tous les fichiers transférés
- Aucune donnée stockée sur un serveur central
- HTTPS obligatoire (fourni automatiquement par Vercel)

## 📝 License

Projet open-source.

## 🆘 Support

Pour l'aide au déploiement, consultez [DEPLOYMENT.md](DEPLOYMENT.md).
