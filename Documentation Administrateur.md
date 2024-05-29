## Installation de Windows Server 2022 avec TightVNC et une connexion SSH

### Prérequis

- *Serveur Windows Server 2022 avec un environnement de bureau installé*
- *Serveur SSH installé et configuré*
- *Logiciel TightVNC installé sur le serveur*
- *Client VNC installé sur la machine locale*

### Choix de l'OS et des logiciels :

### Debian :

**Avantages :**

- Gratuité et open source
- Sécurité et stabilité grâce aux mises à jour régulières de la communauté de sécurité active
- Flexibilité et personnalisation, large choix de logiciels permettant de personnaliser l'installation de sécurité
- Faible consommation de ressources, idéal pour tous types de serveurs

**Inconvénients :**

- Consommation de ressources plus importantes
- Dépendance à l'écosystème Microsoft
- Sous licences et coût assez élevé, mais des ISO de version d'essai existent

### Windows Server 2022 :

**Avantages :**

- Plus de sécurité grâce aux fonctionnalités Secured Core Serveur et Defender For Endpoint
- Meilleur support et maintenance pour les entreprises grâce aux supports Microsoft et à un meilleur écosystème d'entreprises
- Intégration avec les produits Microsoft avec Active Directory, Azure et les autres applications Microsoft telles que Exchange Server, SharePoint ou encore SQL Server

**Inconvénients :**

- Consommation de ressources plus importantes
- Dépendance à l'écosystème Microsoft
- Sous licences et coût assez élevé, mais des ISO de version d'essai existent (nous l'utiliserons car il est mieux adapté pour notre projet)

### RealVNC :

**Avantages :**

- Sécurité renforcée grâce au chiffrement de bout en bout, VNC cloud et 2FA (authentification à deux facteurs)
- Simplicité de l'installation et configurations faciles à faire
- Meilleures performances grâce à l'optimisation de la bande passante et un support multiplateforme (Windows, MacOS, Linux, Android et iOS)
- Fonctionnalités avancées comme la gestion centralisée, le transfert de fichiers et le chat intégré pour communiquer avec les utilisateurs pendant une session
- Support technique performant et documentation très complète
- Adaptation à toutes tailles d'entreprises grâce à des licences flexibles et des fonctionnalités pour des environnements assez importants

**Inconvénients :**

- Licences payantes et coûts assez élevés (c'est la raison pour laquelle on ne l'utilise pas dans ce projet, mais si vous êtes une entreprise, il est conseillé d'utiliser RealVNC)

### TightVNC :

**Avantages :**

- Gratuité et open source, personnalisable selon les besoins (c'est pour cela que l'on utilise durant le projet)
- Simplicité de l'installation et configurations faciles à faire
- Fonctionnalités de base solides pour le contrôle à distance et le transfert de fichiers

**Inconvénients :**

- Moins de fonctionnalités de sécurité comme le chiffrement de bout en bout natif ou le 2FA
- Fonctionnalités limitées comme la gestion centralisée, le chat intégré et d'API pour les intégrations personnalisées
- Mises à jour et maintenance moins fréquentes comparées aux solutions commerciales

## Étapes pour installer Windows Server 2022 avec RealVNC et une connexion SSH :

### Étape 1 : Préparation

1. [Télécharger l'ISO de Windows Server 2022](https://www.microsoft.com/en-us/evalcenter/evaluate-windows-server-2022) : Allez sur le site officiel de Microsoft pour télécharger l'ISO de Windows Server 2022.
2. [Télécharger VirtualBox](https://www.virtualbox.org/wiki/Downloads) : Télécharger et Installer VirtualBox : Téléchargez VirtualBox depuis VirtualBox.org et installez-le. 

### Étape 2 : Création de la VM

1. **Création de la VM :**

   - Ouvrez VirtualBox et cliquez sur "New".
   - Nom : Windows Server 2022.
   - Type : Microsoft Windows.
   - Version : Windows 2022 (64-bit).
   - Mémoire : Attribuez au moins 2048 Mo (2 Go).
   - Disque Dur : Créez un nouveau disque virtuel (VDI) avec une taille dynamique, minimum 50 Go.

2. **Configuration de la VM :**

   - Sélectionnez votre VM et cliquez sur "Settings".
   - Storage : Sous "Controller: IDE", ajoutez le fichier ISO de Windows Server 2022.
   - Network : Configurez l'adaptateur réseau en mode "Bridged" ou "NAT" selon vos besoins.

### Étape 3: Installation de Windows Server 2022

1. **Démarrage de la VM :** Cliquez sur "Start" pour démarrer la VM avec l'ISO. Suivez les instructions pour installer Windows Server 2022, en choisissant les options par défaut à moins que vous ayez des préférences spécifiques.

### Étape 4: Configuration et Installation de TightVNC

1. [Télécharger TightVNC](https://www.tightvnc.com/download.php) : Téléchargez TightVNC pour Windows et installez-le sur votre Windows Server 2022.
2. **Configuration de TightVNC :** Lors de l'installation, configurez un mot de passe pour l'accès VNC. Assurez-vous que TightVNC démarre avec le système.
3. **Ouverture du Port VNC dans le Pare-feu :** Ouvrez le pare-feu Windows Defender. Ajoutez une règle pour autoriser les connexions entrantes sur le port 5900 (port par défaut pour VNC).




  


