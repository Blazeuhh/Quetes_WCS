# Création et Configuration d'un Serveur DHCP sur Windows Server

## Pré-requis

1. **Proxmox VE** installé et configuré.
2. **Windows Server** installé en tant que VM sur Proxmox.
3. **Réseau interne** configuré sur Proxmox pour la VM Windows Server et les machines clientes.

## Étape 1: Configuration du Réseau Interne sur Proxmox

1. **Assigner le réseau interne à la VM Windows Server**:
    - Sélectionnez la VM Windows Server.
    - Allez dans `Hardware`.
    - Ajoutez une nouvelle interface réseau (`Network Device`).
    - Choisissez `vmbr1` comme bridge.

## Étape 2: Installation et Configuration du Rôle DHCP sur Windows Server

1. **Installer le rôle DHCP**:
    - Connectez-vous à votre VM Windows Server.
    - Ouvrez `Server Manager`.
    - Cliquez sur `Add roles and features`.
    - Suivez les instructions pour installer `DHCP Server`.
2. **Configurer le rôle DHCP**:
    - Après l'installation, ouvrez `DHCP Manager` via `Server Manager` > `Tools` > `DHCP`.
    - Dans `DHCP Manager`, faites un clic droit sur votre serveur et sélectionnez `New Scope`.
    - Configurez une nouvelle plage d'adresses :
        - Nom du scope: `Scope_172.20.0.0`.
        - Plage d'adresses: `172.20.0.100` à `172.20.0.200`.
        - Masque de sous-réseau: `255.255.255.0`.
        - Ajoutez les exclusions si nécessaire, sinon laissez par défaut.
        - Configurez les options DHCP telles que la passerelle et le serveur DNS (le cas échéant).
3. **Activer le DHCP**:
    - Une fois la configuration terminée, activez le scope.

## Étape 3: Configuration d'une Réservation DHCP

1. **Obtenir l'adresse MAC de la machine cliente**:
    - Démarrez la machine cliente.
    - Ouvrez une invite de commande ou un terminal et tapez `ipconfig /all` (Windows) ou `ip link show` (Linux) pour trouver l'adresse MAC.
2. **Ajouter une réservation dans DHCP Manager**:
    - Dans `DHCP Manager`, développez votre scope (`Scope_172.20.0.0`).
    - Faites un clic droit sur `Reservations` et sélectionnez `New Reservation`.
    - Entrez le nom de la réservation, l'adresse IP (`172.20.0.10`), l'adresse MAC de la machine cliente et une description (facultatif).
    - Cliquez sur `Add` pour ajouter la réservation.

## Étape 4: Tests

### Test DHCP

1. **Démarrer une machine cliente** (qui n'a pas d'adresse IP statique).
2. **Vérifier l'adresse IP attribuée**:
    - Pour Linux, ouvrez un terminal et tapez:
      ```sh
      ip addr show
      ```
    - Pour Windows, ouvrez une invite de commande et tapez:
      ```sh
      ipconfig
      ```
    - Assurez-vous que l'adresse IP obtenue est dans la plage `172.20.0.100 - 172.20.0.200`.

### Test IP Statique

1. **Configurer une machine cliente avec l'adresse MAC spécifique**.
2. **Redémarrer la machine** et vérifier qu'elle reçoit l'adresse IP `172.20.0.10`:
    - Ouvrez une invite de commande ou un terminal et tapez les commandes mentionnées ci-dessus pour vérifier l'adresse IP attribuée.

