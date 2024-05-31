# Projet 1 - VNC

## **Objectif :** Configurer un accès sécurisé pour l’administration à distance d’un serveur depuis un client.

### Mise en pratique des compétences suivantes :

- Mettre en place un serveur
- Installer et configurer un logiciel/un service
- Réaliser un projet en équipe
- Documenter toutes les étapes
- Faire une démonstration de la réalisation finale

### **Éléments à implémenter :**

- Un client Windows 10 ou Linux Ubuntu
- Un serveur Debian ou Windows Server 2022 (choix : Windows Server 2022)
- Documentations destinées aux administrateurs
- Documentations destinées aux utilisateurs

### Prérequis :

- Serveur Windows Server 2022 avec un environnement de bureau installé
- Serveur SSH installé et configuré
- Logiciel TightVNC installé sur le serveur
- Client VNC installé sur la machine locale

### **Répartitions des rôles :**

#### **Semaine 1 :**

- Mina : Scrum master
- Joris : Product Owner

#### **Semaine 2 :**

- Mohamed : Scrum Master
- Ronan : Product Owner

## Choix de l'OS et des logiciels :

Proxmox :
**Problématique : Nous ne pouvions pas faire communiquer nos machines à distance pour la virtualisation, ce qui nous empêchait de collaborer efficacement sur notre projet.**

**Solution : Proxmox, une plateforme de virtualisation open source, permet de gérer des machines virtuelles sur un serveur dédié. Grâce à Proxmox, nous avons pu collaborer et travailler ensemble sur la même machine virtuelle, facilitant ainsi l'avancement de notre projet.**

### Debian :

**Avantages :**

- Gratuité et open source
- Sécurité et stabilité grâce aux mises à jour régulières de la communauté de sécurité active
- Flexibilité et personnalisation, large choix de logiciels permettant de personnaliser l'installation de sécurité
- Faible consommation de ressources, idéal pour tous types de serveurs

**Inconvénients :**

- Interface utilisateurs qui peut être difficile à appréhender
- Pas beaucoup de compatibilité logicielle comparé à Windows Serveur 2022
- Besoin de connaissances approfondie afin de pouvoir faire des configurations avancées 

### Windows Server 2022 :

**Avantages :**

- Plus de sécurité grâce aux fonctionnalités Secured Core Server et Defender For Endpoint
- Meilleur support et maintenance pour les entreprises grâce aux supports Microsoft et à un meilleur écosystème d'entreprises
- Intégration avec les produits Microsoft avec Active Directory, Azure et les autres applications Microsoft telles que Exchange Server, SharePoint ou encore SQL Server

**Inconvénients :**

- Consommation de ressources plus importantes
- Dépendance à l'écosystème Microsoft
- Sous licences et coût assez élevé, mais des ISO de version d'essai existent (nous l'utiliserons car il est plus adapté pour notre projet)

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










