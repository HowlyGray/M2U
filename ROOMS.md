# Guide des salons publics et privés

## Vue d'ensemble

PeerText est maintenant équipé d'un système de salons publics et privés, permettant :
- 🏛️ **The Street** : Page d'accueil avec liste de salons publics
- 🔒 **Salons privés** : Accessibles uniquement via l'ID
- 🌐 **Salons publics** : Visibles dans The Street
- 📋 **Partage d'ID** : Partagez votre ID pour que d'autres rejoignent vos salons

## Concepts

### Types de salons

#### Salon privé
- **Accessibilité** : Uniquement par son ID (RM-XXXXXX)
- **Visibilité** : Non visible dans The Street
- **Utilisation** : Partagez l'ID avec les personnes de votre choix
- **Idéal pour** : Discussions de groupe privées, conversations personnelles

#### Salon public
- **Accessibilité** : Visible par tous sur The Street
- **Visibilité** : Listé dans la page d'accueil
- **Utilisation** : Rejoignez depuis The Street
- **Idéal pour** : Communautés, discussions ouvertes, événements

### Identifiants

- **PT-XXXXXXXX** : ID PeerJS pour connexion directe P2P
- **RM-XXXXXX** : ID de salon (Room)

## Fonctionnalités

### 1. Page d'accueil "The Street"

La page d'accueil s'affiche automatiquement au démarrage et offre :

#### Boutons d'action principale
- **Créer un salon** : Ouvre le modal de création
- **Rejoindre avec ID** : Pour les salons privés (RM-XXXXXX)
- **Mes salons** : Voir et gérer vos salons créés

#### Liste des salons publics
- Affiche tous les salons publics disponibles
- Cartes avec informations :
  - Nom du salon
  - Nombre de participants
  - Description
  - Créateur
  - Bouton "Rejoindre"
- À terme : Se connectera à un serveur central pour lister les salons

### 2. Modal de création de salon

Permet de créer un nouveau salon avec :

#### Informations requises
- **Nom du salon** (obligatoire, max 30 caractères)
- **Description** (optionnelle, max 200 caractères)

#### Options de visibilité
- **Privé** 🔒 : Salon accessible uniquement via l'ID
  - L'ID du salon sera généré automatiquement (RM-XXXXXX)
  - Partagez cet ID pour inviter des participants
  - Non visible dans The Street

- **Public** 🌐 : Salon visible dans The Street
  - Ajouté à la liste des salons publics
  - Rejoignable par n'importe qui sur The Street
  - Idéal pour les communautés

### 3. Gestion des salons

#### Mes salons
- Liste de tous vos salons créés (publics et privés)
- Actions possibles :
  - Rejoindre un salon
  - Voir les détails
  - Supprimer un salon (à venir)

#### Rejoindre un salon
- **Depuis The Street** : Cliquez sur "Rejoindre" sur une carte de salon public
- **Depuis Mes salons** : Cliquez sur "Rejoindre" sur une de vos salons
- **Avec un ID** : Entrez l'ID (RM-XXXXXX) dans le champ de connexion

## Workflow d'utilisation

### Pour créer un salon public

1. Cliquez sur "Créer un salon"
2. Entrez le nom du salon
3. (Optionnel) Entrez une description
4. Sélectionnez "Public"
5. Cliquez sur "Créer le salon"
6. Le salon est créé et visible dans The Street

### Pour créer un salon privé

1. Cliquez sur "Créer un salon"
2. Entrez le nom du salon
3. (Optionnel) Entrez une description
4. Sélectionnez "Privé"
5. Cliquez sur "Créer le salon"
6. L'ID du salon est généré : **RM-XXXXXX**
7. Partagez cet ID avec les personnes que vous souhaitez inviter

### Pour rejoindre un salon public

1. Sur The Street, cliquez sur "Rejoindre" sur le salon de votre choix
2. Le salon s'ouvre automatiquement
3. Discutez avec les participants connectés

### Pour rejoindre un salon privé

1. Obtenez l'ID du salon (RM-XXXXXX) auprès du créateur
2. Cliquez sur "Rejoindre avec ID"
3. Entrez l'ID du salon
4. Le salon s'ouvre automatiquement
5. Discutez avec les participants connectés

### Pour partager son ID de salon

1. Ouvrez votre salon
2. Copiez votre ID PeerJS (PT-XXXXXXXX)
3. Partagez-le via WhatsApp, Email, SMS, etc.
4. Les destinataires pourront rejoindre en cliquant sur "Rejoindre avec ID" et en entrant votre ID

## Architecture

```
┌─────────────────────────────────────┐
│                                 │
│   THE STREET (Page d'accueil)     │
│                                 │
│  ┌────────────┐  ┌──────────┐  │
│  │ Boutons     │  │ Salons   │  │
│  │ Principaux  │  │ Publics  │  │
│  └────────────┘  └──────────┘  │
│                                 │
│  ┌──────────────────────────────┐  │
│  │ Mes Salons                 │  │
│  │ (Publics + Privés)         │  │
│  └──────────────────────────────┘  │
└─────────────────────────────────────┘
           ↓
    Rejoindre un salon
           ↓
┌─────────────────────────────────────┐
│   CHAT P2P                     │
│                                 │
│  ┌────────────┐  ┌──────────┐  │
│  │ Sidebar    │  │ Chat      │  │
│  │            │  │ Area      │  │
│  └────────────┘  └──────────┘  │
└─────────────────────────────────────┘
```

## Stockage local

### Mes salons
- Stocké dans `localStorage` sous `myRooms`
- Persiste entre les sessions
- Accessible uniquement par l'utilisateur

### Salons publics
- Stocké dans `localStorage` sous `publicRooms`
- À terme : Synchronisé avec un serveur central

### Partage d'ID
- L'ID PeerJS est affiché dans le chat
- Peut être copié via le bouton "Copier mon ID"
- Partagez cet ID pour permettre aux autres de rejoindre

## Sécurité

- ✅ Chiffrement E2E AES-256-GCM pour tous les messages
- ✅ Chiffrement E2E pour tous les fichiers
- ✅ Aucune donnée stockée sur un serveur central (P2P pur)
- ✅ Les salons publics sont visibles mais les contenus restent chiffrés
- ✅ Les identifiants PeerJS sont générés aléatoirement

## À venir

### Fonctionnalités planifiées
- [ ] Serveur central pour synchroniser les salons publics
- [ ] Mise à jour en temps réel du nombre de participants
- [ ] Suppression de salons créés
- [ ] Recherche de salons publics
- [ ] Favoris de salons
- [ ] Historique de messages persistant
- [ ] Notifications de nouveaux messages
- [ ] Modération de salons publics

## Dépannage

### Problème : Je ne peux pas rejoindre un salon public
**Solutions :**
- Vérifiez que vous êtes sur The Street
- Cliquez sur "Rejoindre" sur la carte du salon
- Le champ de connexion doit être vide (laissez The Street gérer l'ID)

### Problème : Mon salon privé ne s'ouvre pas
**Solutions :**
- Vérifiez que vous avez bien entré l'ID complet (RM-XXXXXX)
- L'ID doit être entré dans le champ "ID du pair" sur The Street
- Assurez-vous d'être sur HTTPS si déployé en production

### Problème : Je ne vois pas mes salons dans "Mes salons"
**Solutions :**
- Vérifiez le localStorage du navigateur
- Essayez de créer un nouveau salon pour tester
- Les salons sont stockés localement par navigateur

## Notes techniques

### IDs des salons
- Format : `RM-XXXXXX` (6 caractères alphanumériques)
- Généré avec : `generateId(6)`
- Exemple : `RM-A3B7F9`

### IDs PeerJS
- Format : `PT-XXXXXXXX` (2 lettres + 8 caractères)
- Généré automatiquement par PeerJS
- Utilisé pour les connexions P2P directes

### P2P dans les salons
- Le créateur du salon agit comme "hôte" P2P
- Les autres participants se connectent au créateur
- Les communications restent chiffrées E2E (pair à pair)
- Le créateur relaie les messages aux autres participants
