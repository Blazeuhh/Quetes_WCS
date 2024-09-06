## Exercice 2

## Partie 1 : Gestion des utilisateurs

### Q.2.1.1 : Créer un compte personnel sur le serveur

1. Connectez-vous au serveur en tant qu'utilisateur root ou un utilisateur avec des privilèges sudo.
2. Utilisez la commande suivante pour créer un nouveau compte utilisateur :

   ```
   sudo adduser nom_utilisateur
   ```
   
   Remplacez `nom_utilisateur` par le nom souhaité pour le compte personnel.

4. Suivez les instructions pour définir le mot de passe et configurer les informations utilisateur.

![Capture d'écran 2024-09-06 100050](https://github.com/user-attachments/assets/4a3428ce-0b4a-4a91-8a63-67a7aa3eddee)

![Capture d'écran 2024-09-06 100134](https://github.com/user-attachments/assets/eca3249f-53b7-425c-af01-b3fceac1db95)

### Q.2.1.2 : Préconisations concernant ce compte

1. Voici les préconisations que je propose : 

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
3. Recherchez la ligne suivante :
 
   ```bash
   PermitRootLogin yes
   ```
4. Modifiez-la pour qu'elle soit :

    ```bash
   PermitRootLogin no
   ```
   ![Capture d'écran 2024-09-06 100333](https://github.com/user-attachments/assets/92b2f7a2-f749-4b7e-931a-13aaf2d0af49)

5. Sauvegardez le fichier et quittez l'éditeur.
   
6. Redémarrez le service SSH pour appliquer les changements :
   
   ```bash
   sudo systemctl restart sshd
   ```

### Q.2.2.2 : Autoriser l'accès à distance uniquement à votre compte personnel

1. Dans le fichier `/etc/ssh/sshd_config`, ajoutez ou modifiez la ligne suivante pour spécifier l'utilisateur autorisé :

   ```bash
   AllowUsers nom_utilisateur
   ```
   Remplacez `nom_utilisateur` par votre nom d'utilisateur personnel.

![Capture d'écran 2024-09-06 100546](https://github.com/user-attachments/assets/cffc2817-bf14-465c-bf9e-274dbdbff93c)

2. Sauvegardez et quittez l'éditeur, puis redémarrez le service SSH :

   ```bash
   sudo systemctl restart sshd
   ```

### Q.2.2.3 : Configurer l'authentification par clé SSH et désactiver l'authentification par mot de passe

1. **Générer une paire de clés SSH** sur votre machine locale :

    ```bash
   ssh-keygen -t rsa -b 4096
   ```
3. **Copier la clé publique** sur le serveur :

    ```bash
   ssh-copy-id nom_utilisateur@ip_du_serveur
   ```
5. **Modifier la configuration SSH** sur le serveur :
 
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
La commande suivante permet de lister les systèmes de fichiers actuellement montés :
```bash
df -hT
```
- Voici les systèmes de fichiers actuellement montés : 

![Capture d'écran 2024-09-06 101426](https://github.com/user-attachments/assets/96f58e9e-c36e-4ae7-b522-0c2117613625)

### Q.2.3.2 : Identifier le type de système de stockage utilisé

- Les systèmes de stockage sont dans mon cas : devtmpfs, tmpfs, ext4, ext2.

### Q.2.3.3 : Ajouter un nouveau disque et réparer le volume RAID

1. **Ajouter le nouveau disque sur votre hyperviseur de 8 gb** .

2. **Identifier le disque** :

   ```bash
   sudo fdisk -l
   ```
4. **Ajouter le disque au RAID** :

    - Identifiez le nom du périphérique RAID :

   ```bash
     cat /proc/mdstat
     ```
   - Ajoutez le disque au RAID (par exemple, pour `/dev/md0`) :

      ```bash
     sudo mdadm --manage /dev/md0 --add /dev/sdXx
     ```
   - Remplacez `/dev/sdXx` par le nouveau disque. (sda1 dans cas)

### Q.2.3.4 : Créer un volume logique LVM de 2 Gio pour les sauvegardes

1. **Créer un volume logique** de 2 Gio :

   ```bash
   sudo lvcreate -L 2G -n backup_storage nom_groupe_de_volume
   ```
3. **Formater le volume** :

   ```bash
   sudo mkfs.ext4 /dev/nom_groupe_de_volume/backup_storage
   ```
4. **Monter le volume** :

   ```bash
   sudo mkdir -p /var/lib/bareos/storage
   sudo mount /dev/nom_groupe_de_volume/backup_storage /var/lib/bareos/storage
   ```
5. **Modifier `/etc/fstab`** pour monter automatiquement le volume au démarrage :

   ```bash
   echo '/dev/nom_groupe_de_volume/backup_storage /var/lib/bareos/storage ext4 defaults 0 2' | sudo tee -a /etc/fstab
   ```

### Q.2.3.5 : Vérifier l'espace disponible dans le groupe de volumes

Pour vérifier l'espace disponible dans le groupe de volumes LVM :

```bash
sudo vgdisplay nom_groupe_de_volume
```

![Capture d'écran 2024-09-06 111952](https://github.com/user-attachments/assets/0f262e74-8a1f-47cd-81bb-4eb913449e8d)

- Ici on peut voir qu'il me reste 1,79GB
  
## Partie 4 : Sauvegardes

### Q.2.4.1 : Explication des composants Bareos

- **bareos-dir (Director)** : Il orchestre les sauvegardes, envoie des instructions au Storage Daemon (SD) et au File Daemon (FD), et gère la base de données des tâches de sauvegarde.
- **bareos-sd (Storage Daemon)** : Gère les supports de stockage (disques, bandes) et reçoit les données à sauvegarder du File Daemon (FD).
- **bareos-fd (File Daemon)** : Installe sur les clients, il s'occupe d'envoyer les données à sauvegarder au Storage Daemon (SD).

## Partie 5 : Filtrage et analyse réseau

### Q.2.5.1 : Afficher les règles Netfilter actuelles

Pour afficher les règles Netfilter actuelles :
```bash
sudo nft list ruleset
```

### Q.2.5.2 : Communications autorisées

![Capture d'écran 2024-09-06 103050](https://github.com/user-attachments/assets/d70ed892-0da4-4b63-b224-0a0348424c57)

Voici les communcations autorisé :

1. ct state etablished,related accept
2. lifname"lo" accept
3. tcp dport 22 accept
4. ip protocol icmp accept
5. ip6 nexthdr ipv6-icmp accept


### Q.2.5.3 : Communications interdites

![Capture d'écran 2024-09-06 103050](https://github.com/user-attachments/assets/d70ed892-0da4-4b63-b224-0a0348424c57)

Voici les communcations interdites :

1. type filter hook input priority filter; policy drop
2. ct state invalid drop

### Q.2.5.4 : Ajouter des règles pour Bareos dans nftables

1. **Éditer la configuration nftables** :

    ```bash
   sudo nano /etc/nftables.conf
   ```
3. **Ajouter les règles pour autoriser Bareos** :

   ```bash
   table inet filter {
       chain input {
           type filter hook input priority 0;
           tcp dport { 9101, 9102, 9103 } accept;
       }
   }
   ```
![Capture d'écran 2024-09-06 103437](https://github.com/user-attachments/assets/eafaa05e-1cf8-4e1f-9666-5fab62c512f6)

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
![Capture d'écran 2024-09-06 103549](https://github.com/user-attachments/assets/e887cc2d-9bca-4c4e-9e7b-8bb5240b0a9b)

