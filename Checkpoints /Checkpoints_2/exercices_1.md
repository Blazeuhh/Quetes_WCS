### Q.1.1 Ping IPv4 entre le serveur et le client

Nous voyons ici que le ping sur l'IPv4 du serveur `172 .16.10.10` ne fonctionne pas (pourtant le serveur et le client sont en réseau interne) :

![Capture d'écran 2024-07-19 091234](https://github.com/user-attachments/assets/217d3577-c9b6-49eb-86e7-51ca616c16c6)

Nous allons voir pourquoi il y a ce problème , pour ce faire : 

 - Allez dans le **Panneau de configuration** puis dans **Internet et réseaux**, **Centre Réseau et Partage** et **Modifier les paramètres de l’adaptateur**.
 - Puis séléctionner votre carte réseau et séléctionner **Propriétés**, cliquez sur **Protocole Internet version 4 (TCP/IPV4)**.

![Capture d'écran 2024-07-19 091806](https://github.com/user-attachments/assets/bd51bcc6-82bf-4e2e-992f-87d4df07a359)

- Changez à présent l'adresse IP de votre ordinateur par `172.16.10.11`, laissez le Masque de sous-réseaux sur `255.255.255.0`.
- Mettez pour la passerelle et le DNS l'IP sur serveur qui est `172.16.10.11`.

A présent, vous pouvez réesayez le ping qui devvrait désormais fonctionner :

![Capture d'écran 2024-07-19 104335](https://github.com/user-attachments/assets/93ad3edd-3fb3-4997-885d-d38af5c4562e)


### Q.1.3 Désactivation du protocole IPv6 et vérification

1. **Ouvrir le Centre Réseau et Partage**

   - Faites un clic droit sur l'icône réseau dans la barre des tâches et sélectionnez **Ouvrir les paramètres de réseau et Internet**.
   - Dans la fenêtre qui s'ouvre, cliquez sur **Centre Réseau et Partage**.
   - Cliquez sur **Modifier les paramètres de l’adaptateur** .
   - Décochez la case à côté de **Protocole Internet version 6 (TCP/IPv6)**.
   - Cliquez sur **OK** pour appliquer les modifications.

![Capture d'écran 2024-07-19 092346](https://github.com/user-attachments/assets/30ae57c9-c3bd-43ef-b9c2-43e9641e9822)

![Capture d'écran 2024-07-19 092610](https://github.com/user-attachments/assets/020b0e21-c8e3-4df4-a794-80d241a1fd51)

### Q.1.4 Configuration DHCP

![Capture d'écran 2024-07-19 093027](https://github.com/user-attachments/assets/358c4c44-8b1b-400e-9f04-1871780bde19)

![Capture d'écran 2024-07-19 093231](https://github.com/user-attachments/assets/e33ee5fd-c0a9-485b-9d4a-15b14d1e5c2f)

![Capture d'écran 2024-07-19 093326](https://github.com/user-attachments/assets/b88583ce-66e4-4145-8179-362bfc6f9d8f)

![Capture d'écran 2024-07-19 093706](https://github.com/user-attachments/assets/c0dfd53f-7735-47be-aef9-a7ea52ac845d)

![Capture d'écran 2024-07-19 095045](https://github.com/user-attachments/assets/5606d196-1598-401d-afc8-38e396802082)

### Q.1.5 Pourquoi le client ne récupère pas la 1ère adresse disponible ?

- Les serveurs DHCP utilisent souvent des mécanismes pour éviter l'attribution de la première adresse d'une plage pour éviter les conflits avec des adresses statiques ou pour réserver certaines adresses.
  
![Capture d'écran 2024-07-19 095101](https://github.com/user-attachments/assets/a559a09a-fda6-470b-b0b3-65ab7a0bff75)

### Q.1.6 Configuration DHCP pour une adresse IP fixe

![Capture d'écran 2024-07-19 101934](https://github.com/user-attachments/assets/c3b47b62-215a-4476-a783-08a0ddbfa658)

### Q.1.7 Pourquoi passez ce réseau en IPV6?

- Passer à IPv6 n'est pas simplement une réponse à l'épuisement des adresses IPv4. C'est une avancée vers une infrastructure réseau plus sécurisée, efficace et capable de supporter l'énorme croissance des dispositifs connectés et des nouvelles technologies. Bien que la transition nécessite un investissement en termes de mise à jour des équipements et des compétences, les bénéfices à long terme rendent IPv6 essentiel pour l'évolution des réseaux modernes.

### Q.1.8 Serveur DHCP obsolète?

- Non, le serveur DHCP n'est pas obsolète dans un réseau IPv6. Il continue à jouer un rôle important en gérant les adresses IP et en fournissant des informations de configuration supplémentaires aux clients. En IPv6, le DHCP est connu sous le nom de DHCPv6. 

Voici les réponses détaillées à chaque question posée sur la configuration réseau et la gestion des adresses IP pour un réseau avec des machines virtuelles (VM).



**Montre et explique le résultat d'un ping IPv4 du serveur vers le client.**

1. **Configuration Initiale des IP (exemple)** :
   - Serveur : 192.168.1.1
   - Client : 192.168.1.2

2. **Vérifie la connectivité avec un ping** :
   - Sur le serveur, exécute la commande :
     ```bash
     ping 192.168.1.2
     ```

3. **Interprétation des Résultats** :
   - Si le ping échoue, cela peut indiquer un problème de configuration réseau, de pare-feu ou de connexion physique.

4. **Modification de la configuration IP sur le client** :
   - Vérifie la configuration IP sur le client avec :
     ```bash
     ifconfig
     ```
   - Si le client n'a pas la bonne adresse IP ou est mal configuré, il faut la corriger. Supposons qu'on utilise Linux, la commande pour changer l'adresse IP pourrait être :
     ```bash
     sudo ifconfig eth0 192.168.1.2 netmask 255.255.255.0
     ```

5. **Ping fonctionnel après modification** :
   - Après avoir ajusté la configuration, réessaie le ping depuis le serveur :
     ```bash
     ping 192.168.1.2
     ```

**Exemple de résultat de ping réussi** :
   ```
   PING 192.168.1.2 (192.168.1.2) 56(84) bytes of data.
   64 bytes from 192.168.1.2: icmp_seq=1 ttl=64 time=0.123 ms
   ```

### Q.1.2 Ping avec les noms des machines

1. **Configurer les fichiers hosts (si nécessaire)** :
   - Sur le serveur, édite `/etc/hosts` pour ajouter une entrée pour le client :
     ```
     192.168.1.2 client
     ```

   - Sur le client, édite `/etc/hosts` pour ajouter une entrée pour le serveur :
     ```
     192.168.1.1 serveur
     ```

2. **Ping en utilisant les noms** :
   - Sur le serveur, exécute :
     ```bash
     ping client
     ```

   - Sur le client, exécute :
     ```bash
     ping serveur
     ```

**Explication des résultats** :
   - Si les pings sont réussis, cela confirme que la résolution de noms fonctionne correctement.



1. **Désactiver IPv6 sur le client** :
   - Sur Linux, modifie les paramètres sysctl :
     ```bash
     sudo sysctl -w net.ipv6.conf.all.disable_ipv6=1
     sudo sysctl -w net.ipv6.conf.default.disable_ipv6=1
     sudo sysctl -w net.ipv6.conf.lo.disable_ipv6=1
     ```

2. **Ping avec les noms après désactivation IPv6** :
   - Réessaie le ping avec le nom :
     ```bash
     ping serveur
     ```

**Explication du résultat** :
   - Si le ping échoue après désactivation d'IPv6, cela signifie que le DNS ou les noms de machines utilisaient IPv6. 

3. **Modification pour que le ping fonctionne** :
   - Si la résolution de noms en IPv4 est le problème, assure-toi que les fichiers `/etc/hosts` sont correctement configurés.

**Modification et ping fonctionnel** :
   - Vérifie que les fichiers `hosts` sont bien configurés et refais le ping.

### Q.1.4 Configuration DHCP

1. **Configurer le serveur DHCP** :
   - Modifie le fichier de configuration du serveur DHCP (`/etc/dhcp/dhcpd.conf`) pour définir une plage d'adresses :
     ```
     subnet 192.168.1.0 netmask 255.255.255.0 {
       range 192.168.1.10 192.168.1.50;
       option routers 192.168.1.1;
       option domain-name-servers 8.8.8.8;
     }
     ```

2. **Configurer le client pour utiliser DHCP** :
   - Sur le client, configure l'interface réseau pour obtenir une IP via DHCP :
     ```bash
     sudo dhclient eth0
     ```



1. **Explication** :
   - Les serveurs DHCP utilisent souvent des mécanismes pour éviter l'attribution de la première adresse d'une plage pour éviter les conflits avec des adresses statiques ou pour réserver certaines adresses.

2. **Copie d'écran** :
   - Pour voir l'adresse IP que le client a obtenue, vérifie avec :
     ```bash
     ifconfig
     ```



1. **Configurer une adresse IP fixe pour un client spécifique** :
   - Modifie le fichier de configuration DHCP pour ajouter une réservation :
     ```
     host client {
       hardware ethernet 00:11:22:33:44:55;
       fixed-address 172.16.10.15;
     }
     ```

2. **Redémarre le serveur DHCP** :
   ```bash
   sudo systemctl restart isc-dhcp-server
   ```

**Montre tes modifications** :
   - Modifie le fichier de configuration comme ci-dessus et redémarre le service DHCP.




       }
       ```

   - Redémarre le service :
     ```bash
     sudo systemctl restart isc-dhcp-server
     ```

Cette réponse couvre les principales questions et fournit les configurations nécessaires pour chaque étape. Assure-toi d’adapter les adresses IP, les noms de fichiers et les interfaces réseau selon ton environnement spécifique.
