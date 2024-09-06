Pour répondre aux différentes questions de l'exercice, voici les étapes détaillées pour chaque partie :

## Partie 1 : Gestion des utilisateurs

### Q.2.1.1 : Créer un compte personnel sur le serveur
Pour créer un compte personnel sur un serveur Linux :

1. Connectez-vous au serveur en tant qu'utilisateur root ou un utilisateur avec des privilèges sudo.
2. Utilisez la commande suivante pour créer un nouveau compte utilisateur :
   ```bash
   sudo adduser nom_utilisateur
   ```
   Remplacez `nom_utilisateur` par le nom souhaité pour le compte personnel.
3. Suivez les instructions pour définir le mot de passe et configurer les informations utilisateur.

### Q.2.1.2 : Préconisations concernant ce compte
- **Utilisation d’un mot de passe fort** : Assurez-vous que le mot de passe du compte est complexe, comprenant des lettres majuscules, des minuscules, des chiffres et des caractères spéciaux.
- **Accès sudo restreint** : Accordez des privilèges sudo uniquement si nécessaire pour limiter les risques de modification accidentelle ou malveillante du système.
- **Authentification par clé SSH** : Configurez l'authentification par clé SSH pour renforcer la sécurité.
- **Désactivation du mot de passe pour SSH** : Désactivez l'accès SSH par mot de passe pour éviter les attaques par force brute.

## Partie 2 : Configuration de SSH

### Q.2.2.1 : Désactiver l'accès à distance de l'utilisateur root
1. Éditez le fichier de configuration SSH :
   ```bash
   sudo nano /etc/ssh/sshd_config
   ```
2. Recherchez la ligne suivante :
   ```bash
   PermitRootLogin yes
   ```
3. Modifiez-la pour qu'elle soit :
   ```bash
   PermitRootLogin no
   ```
4. Sauvegardez le fichier et quittez l'éditeur.
5. Redémarrez le service SSH pour appliquer les changements :
   ```bash
   sudo systemctl restart sshd
   ```

### Q.2.2.2 : Autoriser l'accès à distance uniquement à votre compte personnel
1. Dans le fichier `/etc/ssh/sshd_config`, ajoutez ou modifiez la ligne suivante pour spécifier l'utilisateur autorisé :
   ```bash
   AllowUsers nom_utilisateur
   ```
   Remplacez `nom_utilisateur` par votre nom d'utilisateur personnel.

2. Sauvegardez et quittez l'éditeur, puis redémarrez le service SSH :
   ```bash
   sudo systemctl restart sshd
   ```

### Q.2.2.3 : Configurer l'authentification par clé SSH et désactiver l'authentification par mot de passe
1. **Générer une paire de clés SSH** sur votre machine locale (si ce n'est pas déjà fait) :
   ```bash
   ssh-keygen -t rsa -b 4096
   ```
2. **Copier la clé publique** sur le serveur :
   ```bash
   ssh-copy-id nom_utilisateur@ip_du_serveur
   ```
3. **Modifier la configuration SSH** sur le serveur :
   - Éditez `/etc/ssh/sshd_config` et assurez-vous que les lignes suivantes sont présentes et configurées ainsi :
     ```bash
     PasswordAuthentication no
     PubkeyAuthentication yes
     ```
   - Sauvegardez et quittez l'éditeur.
   - Redémarrez le service SSH :
     ```bash
     sudo systemctl restart sshd
     ```

## Partie 3 : Analyse du stockage

### Q.2.3.1 : Lister les systèmes de fichiers montés
Utilisez la commande suivante pour lister les systèmes de fichiers actuellement montés :
```bash
df -hT
```

### Q.2.3.2 : Identifier le type de système de stockage utilisé
devtmpfs, tmpfs,ext4,ext2

### Q.2.3.3 : Ajouter un nouveau disque et réparer le volume RAID
1. **Ajouter le nouveau disque** (fait dans un hyperviseur ou via un script).
2. **Identifier le disque** :
   ```bash
   sudo fdisk -l
   ```
3. **Ajouter le disque au RAID** :
   - Identifiez le nom du périphérique RAID :
     ```bash
     cat /proc/mdstat
     ```
   - Ajoutez le disque au RAID (par exemple, pour `/dev/md0`) :
     ```bash
     sudo mdadm --manage /dev/md0 --add /dev/sdX
     ```
   - Remplacez `/dev/sdX` par le nouveau disque.

### Q.2.3.4 : Créer un volume logique LVM de 2 Gio pour les sauvegardes
1. **Créer un volume logique** de 2 Gio :
   ```bash
   sudo lvcreate -L 2G -n backup_storage nom_groupe_de_volume
   ```
2. **Formater le volume** :
   ```bash
   sudo mkfs.ext4 /dev/nom_groupe_de_volume/backup_storage
   ```
3. **Monter le volume** :
   ```bash
   sudo mkdir -p /var/lib/bareos/storage
   sudo mount /dev/nom_groupe_de_volume/backup_storage /var/lib/bareos/storage
   ```
4. **Modifier `/etc/fstab`** pour monter automatiquement le volume au démarrage :
   ```bash
   echo '/dev/nom_groupe_de_volume/backup_storage /var/lib/bareos/storage ext4 defaults 0 2' | sudo tee -a /etc/fstab
   ```

### Q.2.3.5 : Vérifier l'espace disponible dans le groupe de volumes
Pour vérifier l'espace disponible dans le groupe de volumes LVM :
```bash
sudo vgdisplay nom_groupe_de_volume
```

## Partie 4 : Sauvegardes

### Q.2.4.1 : Explication des composants Bareos
- **bareos-dir (Director)** : Il orchestre les sauvegardes, envoie des instructions au Storage Daemon (SD) et au File Daemon (FD), et gère la base de données des tâches de sauvegarde.
- **bareos-sd (Storage Daemon)** : Gère les supports de stockage (disques, bandes) et reçoit les données à sauvegarder du File Daemon (FD).
- **bareos-fd (File Daemon)** : Installe sur les clients, il s'occupe d'envoyer les données à sauvegarder au Storage Daemon (SD).

## Partie 5 : Filtrage et analyse réseau

### Q.2.5.1 : Afficher les règles Netfilter actuelles
Pour afficher les règles Netfilter actuelles :
```bash
sudo iptables -L -v -n
```

### Q.2.5.2 : Communications autorisées
Les règles affichées par la commande précédente vous indiqueront quelles communications sont autorisées en fonction des chaînes de règles (`INPUT`, `FORWARD`, `OUTPUT`).

### Q.2.5.3 : Communications interdites
De même, les règles de filtrage indiqueront quelles communications sont bloquées ou interdites.

### Q.2.5.4 : Ajouter des règles pour Bareos dans nftables
1. **Éditer la configuration nftables** :
   ```bash
   sudo nano /etc/nftables.conf
   ```
2. **Ajouter les règles pour autoriser Bareos** :
   ```bash
   table inet filter {
       chain input {
           type filter hook input priority 0;
           tcp dport { 9101, 9102, 9103 } accept;
       }
   }
   ```
3. **Appliquer les règles** :
   ```bash
   sudo nft -f /etc/nftables.conf
   ```

## Partie 6 : Analyse de logs

### Q.2.6.1 : Lister les 10 derniers échecs de connexion
1. Utilisez la commande suivante pour extraire les 10 derniers échecs de connexion :
   ```bash
   sudo grep "Failed password" /var/log/auth.log | tail -n 10
   ```
2. Pour chaque ligne, vous verrez la date, l'heure, et l'adresse IP de la tentative échouée. Vous pouvez raffiner la commande pour extraire spécifiquement ces informations.
