
## Installation de Windows Serveur 2022 avec un TightVNC et une connexion SSH **

### Prérequis

#### *- Serveur Windows Serveur 2022 avec un environnement de bureau installé.*
#### *- Serveur SSH installé et configuré.*
#### *- Logiciel RealVNC installé sur le serveur.*
#### *- Client VNC installé sur la machine locale .*

### Choix de l'OS et des logiciels :

  - Debian :

    Avantages :
   
     - Gratuité et open source
     - Sécurité et stabilité grâce aux mis à jour régulières de la communautés de sécurité active
     - Flexibilité et personnalisation, large choix de logiciels permettant de personnaliser l'installation de sécurité
     - Faible consommation de ressources, idéal pour tout types de serveurs
  
   Inconvénients :
     
     - Consommation de ressources plus importantes
     - Dépendance à l'écosystème Microsoft
     - Sous licences et coût assez élevé , mais des iso de version d'essai existe
 
 
- Windows Serveur 2022 :

   Avantages :
   
     - Plus de sécurité grâces aux fonctionnalités Secured Core Serveur et Defender For Endpoint
     - Meilleur support et maintenance pour les entreprises grâce aux supports Microsoft et à un meilleur écosystème d'entreprises.
     - Intégration avec les produits Microsoft avec Active Directory, Azure et les autres applications Microsoft tel que Exchange Server, Sharepoint ou encore SQL serveur
  
   Inconvénients :
     - Consommation de ressources plus importantes
     - Dépendance à l'écosystème Microsoft
     - Sous licences et coût assez élevé , mais des iso de version d'essai existe ( nous l'utliserons car il est mieux adapté pour notre projet )
  

 
 - Real VNC :

   Avantages :
   
     - Sécurité renforcée grâce aux chiffrement de bout en bout, VNC cloud et 2FA (authentification à deux facteurs)
     - Simplicité de l'installation et les configurations sont faciles à faire
     - De meilleurs performances grâce à l'optimisation de la bande passante et un support multiplateforme (Windows, MacOS, Linux, Android, et IOS)
     - Fonctionnalités avancées comme la gestion centralisée, le transfert de fichiers et le chat intégré pour communiquer avec les utilisateurs pendant une session.
     - Support technique performant et la documentation est très complètes
     - Adaptation à toutes tailles d'entreprises grâce à licences flexibles et des fonctionnalités à des envrionnements assez importants

   Inconvénients :    
     
     - Licence payantes et coûts assez élevé( c'est la raison de pourquoi on l'utilise pas dans ce projet, mais si vous êtes une entreprise , il est conseiller d'utiliser RealVNC )
  
  - Tight VNC :

    Avantages :
   
     - Gratuité et open source, personalisable selon les besoins ( c'est pour cela que l'on utilise durant le projet )
     - Simplicité de l'installation et les configurations sont faciles à faire
     - Fonctionnalités de bases solides pour le contrôles à distance et le transfert de fichiers est disponibke
    
    Inconvénients :    
     
     - Moins de fonctionnalités de sécurité comme le chiffrement de bout en bout natif ou le 2FA
     - Fonctionnalités limités comme la gestion centralisée, le chat intégré et d'API pour les intégrations personnalisées
     - Mis à jour et maintenance moins fréquentes comparés aux solutions commerciales
       
       
## Etapes pour installer Windows Serveur 2022 avec un RealVNC et une connexion SSH :

### Etape 1 : Préparation

  1. [Télécharger l'ISO de Windows Server 2022](https://www.microsoft.com/en-us/evalcenter/evaluate-windows-server-2022)
Allez sur le site officiel de Microsoft pour télécharger l'ISO de Windows Server 2022. 

  2. Télécharger et Installer VirtualBox :
Téléchargez VirtualBox depuis VirtualBox.org et installez-le. [Télécharger VirtualBox](https://www.virtualbox.org/wiki/Downloads)

### Etape 2 : Création de la VM

  1. Création de la VM

  - Ouvrez Virtual Box et cliquez sur "New"
  - Nom : Windows Server 2022.
  - Type : Microsoft Windows.
  - Version : Windows 2022 (64-bit).
  - Mémoire : Attribuez au moins 2048 MB (2 GB).
  - Disque Dur : Créez un nouveau disque virtuel (VDI) avec une taille dynamique,
minimum 50 GB

  2. Configuration de la VM :

  - Sélectionnez votre VM et cliquez sur "Settings".
  - Storage : Sous "Controller: IDE", ajoutez le fichier ISO de Windows Server 2022.
  - Network : Configurez l'adaptateur réseau en mode "Bridged" ou "NAT" selon vos
besoins.

### Étape 3: Installation de Windows Server 2022

  1. Démarrage de la VM :
Cliquez sur "Start" pour démarrer la VM avec l'ISO.
Suivez les instructions pour installer Windows Server 2022, en choisissant les
options par défaut à moins que vous ayez des préférences spécifiques.


### Étape 4: Configurer et Installer TightVNC

  1. [Télécharger TightVNC](https://www.tightvnc.com/download.php)
Téléchargez TightVNC pour Windows et installez-le sur votre Windows Server 2022. 
  2. Configuration de TightVNC :
Lors de l'installation, configurez un mot de passe pour l'accès VNC.
Assurez-vous que TightVNC démarre avec le système.
  3. Ouverture du Port VNC dans le Pare-feu :
Ouvrez le pare-feu Windows Defender.
Ajoutez une règle pour autoriser les connexions entrantes sur le port 5900 (port par
défaut pour VNC).




  


