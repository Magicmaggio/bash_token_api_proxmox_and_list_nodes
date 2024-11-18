# Automatisation de l'Accès à l'API Proxmox avec un Script Bash

Ce script Bash permet d'automatiser l'obtention d'un jeton d'accès à l'API Proxmox VE et de récupérer la liste des nœuds d'un serveur Proxmox. Les informations sensibles (comme l'adresse IP, le nom d'utilisateur, et le mot de passe) sont stockées dans un fichier .env pour garantir la sécurité et la facilité d'utilisation.  

## Prérequis

 - Proxmox VE installé et fonctionnel avec une API accessible.
 - curl et jq installés sur votre machine pour effectuer des requêtes HTTP et manipuler les réponses JSON.
 - Fichier .env contenant les variables sensibles (utilisateur, mot de passe, etc.).

## Structure du Projet
```
/mon-repertoire
│
├── script.sh        # Script principal pour obtenir le jeton et récupérer les nœuds
├── .env             # Fichier contenant les variables sensibles (ex. : PROXMOX_HOST, PROXMOX_USERNAME)
└── .gitignore       # Assurez-vous d'ajouter .env dans ce fichier pour éviter de pousser le fichier vers GitHub
```

## Variables d'Environnement dans .env

Créez un fichier .env dans le même répertoire que votre script et ajoutez-y les informations suivantes :
```bash
# .env
PROXMOX_HOST="<ip_serveur_proxmox>:8006"
PROXMOX_USERNAME="root@pam"
PROXMOX_PASSWORD="..."
```

### Explication des Variables :

 - PROXMOX_HOST : L'adresse IP et le port de votre serveur Proxmox (ex. <ip_serveur_proxmox>:8006).
 - PROXMOX_USERNAME : Le nom d'utilisateur pour se connecter à l'API Proxmox (par exemple, root@pam).
 - PROXMOX_PASSWORD : Le mot de passe associé à l'utilisateur Proxmox.

### Comment Utiliser le Script

 - Préparer votre Environnement :
     - Téléchargez ou clonez le projet sur votre machine.
     - Assurez-vous que le fichier .env est configuré avec les bonnes valeurs pour votre serveur Proxmox.

 - Exécuter le Script :
     - Ouvrez un terminal et naviguez jusqu'au répertoire contenant le script.
     - Assurez-vous que le script est exécutable :

``chmod +x script.sh``

### Exécutez le script :

``./script.sh``

Affichage des Résultats :
 - Le script obtient un jeton d'accès à l'API Proxmox et l'affiche dans le terminal.
 - Il récupère également la liste des nœuds disponibles sur le serveur Proxmox et les affiche.

**Exemple de sortie du script :**
```bash
Obtention du jeton d'accès...
Jeton d'accès (ticket) : PVE:root@pam:673B09F2::...
Token CSRFPrevention : 673B09F2:xHTkkzhiD0dW/QZumn/GIcvH1/Zn/hWRN1q...
Récupération de la liste des nœuds...
Liste des nœuds disponibles :
lab
```
## Gestion des Secrets avec `.gitignore

**Ne jamais inclure votre fichier .env dans un dépôt Git public ou privé pour éviter de partager des informations sensibles comme les mots de passe et les noms d'utilisateur.**

 - Ajoutez .env au fichier .gitignore :

Créez un fichier .gitignore dans le répertoire de votre projet (si ce n'est pas déjà fait) et ajoutez-y .env :
```bash
# .gitignore
.env
```
## Sécuriser et Déployer

 - Stockage sécurisé des secrets : Utilisez des gestionnaires de secrets comme AWS Secrets Manager ou Vault pour gérer vos secrets dans des environnements de production.
 - Autres utilisateurs : D'autres utilisateurs peuvent cloner ce dépôt, créer leur propre fichier .env et exécuter le script sans partager les informations sensibles.

FAQ

Que faire si mon jeton expire ?
 - Le jeton d'accès est valide pendant 2 heures. Si vous ne faites pas de requêtes avant son expiration, vous devrez générer un nouveau jeton en exécutant à nouveau le script.

Comment vérifier si mon jeton est valide ?
 - Si le jeton est invalide, vous recevrez une réponse d'erreur de l'API, et le script affichera un message d'erreur.

Puis-je utiliser un API Token pour l'authentification ?
 - Oui, pour un accès plus sécurisé et persistant, vous pouvez configurer un API Token dans l'interface web de Proxmox. Remplacez le nom d'utilisateur et le mot de passe par votre token dans le fichier .env.