# M2U (PeerText)

Une interface gratuite de discussion peer-to-peer chiffrée de bout en bout avec système de salons publics et privés.

## 🚀 Fonctionnalités

- ✅ **Chiffrement E2E AES-256-GCM** : Messages et fichiers sécurisés
- ✅ **WebRTC P2P** : Communication directe sans serveur intermédiaire
- ✅ **Transfert de fichiers** : Partage de fichiers jusqu'à 100 MB
- ✅ **Salons publics** : Liste des salons visibles par tous sur "The Street"
- ✅ **Salons privés** : Salons accessibles uniquement via l'ID
- ✅ **Page d'accueil "The Street"** : Découvrez et rejoignez des salons
- ✅ **Création de salons** : Créez des salons publics ou privés
- ✅ **Partage d'ID** : Partagez votre ID pour que d'autres rejoignent vos salons
- ✅ **Multi-utilisateurs** : Support des salons de discussion
- ✅ **Indicateur de frappe** : Voyez quand quelqu'un écrit
- ✅ **Interface responsive** : Design moderne et adapté mobile
- ✅ **Configuration ICE** : Serveurs STUN/TURN personnalisables
- ✅ **Prêt pour internet** : Fonctionne sur différents réseaux via TURN servers

## 🏛️ The Street - Page d'accueil

The Street est la page d'accueil qui s'affiche au démarrage de l'application.

### Fonctionnalités
- **Créer un salon** : Créez un nouveau salon (public ou privé)
- **Rejoindre avec ID** : Rejoignez un salon privé via son ID (RM-XXXXXX)
- **Mes salons** : Gérez vos salons créés
- **Salons publics** : Découvrez les salons publics disponibles

### Types de salons
- **🔒 Salon privé** : Accessible uniquement via l'ID généré (RM-XXXXXX)
  - Idéal pour discussions privées et groupes fermés
  - Partagez l'ID avec les personnes de votre choix
  
- **🌐 Salon public** : Visible par tous sur The Street
  - Idéal pour communautés et discussions ouvertes
  - Rejoignable directement depuis la liste

## 📋 Identifiants

- **PT-XXXXXXXX** : ID PeerJS pour connexion directe P2P
- **RM-XXXXXX** : ID de salon (Room) pour rejoindre un salon

Pour plus de détails sur le système de salons, consultez [ROOMS.md](ROOMS.md).

## 🌐 Déploiement sur Vercel

### Méthode rapide (CLI)

```bash
npm install -g vercel
vercel login
vercel
```

### Méthode via l'interface web

1. Poussez votre code sur GitHub/GitLab/Bitbucket
2. Importez le projet sur [vercel.com](https://vercel.com)
3. Déployez en un clic

Pour des instructions détaillées, voir [DEPLOYMENT.md](DEPLOYMENT.md).

## 🔧 Configuration des serveurs ICE

L'application inclut déjà des serveurs STUN/TURN gratuits (OpenRelay) pour fonctionner sur internet.

Pour personnaliser les serveurs :

1. Cliquez sur le bouton ⚙️ (Paramètres) dans le coin supérieur droit
2. Ajoutez des serveurs STUN/TURN personnalisés
3. La configuration est sauvegardée localement dans le navigateur

### Serveurs TURN pour production

Pour un usage en production avec un serveur TURN dédié :

- **Auto-hébergé** : Utilisez [coturn](https://github.com/coturn/coturn) sur un VPS (coût: ~5€/mois)
- **Services cloud** : Twilio, Xirsys, Metered TURN
- Configurer des serveurs TURN géographiquement proches des utilisateurs

## 🔒 Sécurité

- Chiffrement de bout en bout pour tous les messages
- Chiffrement de tous les fichiers transférés
- Aucune donnée stockée sur un serveur central
- Les communications restent P2P et chiffrées
- HTTPS obligatoire (fourni automatiquement par Vercel)

## 📝 License

Projet open-source.

## 🆘 Support

- Pour l'aide sur les salons : [ROOMS.md](ROOMS.md)
- Pour l'aide au déploiement : [DEPLOYMENT.md](DEPLOYMENT.md)
- Pour les améliorations : [IMPROVEMENTS.md](IMPROVEMENTS.md)
