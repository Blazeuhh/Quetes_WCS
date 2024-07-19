### Ping du du client au serveur et correction des problèmes

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


### Désactiver IPV6 et ping 

1. **Ouvrir le Centre Réseau et Partage**

   - Faites un clic droit sur l'icône réseau dans la barre des tâches et sélectionnez **Ouvrir les paramètres de réseau et Internet**.
   - Dans la fenêtre qui s'ouvre, cliquez sur **Centre Réseau et Partage**.
   - Cliquez sur **Modifier les paramètres de l’adaptateur** .
   - Décochez la case à côté de **Protocole Internet version 6 (TCP/IPv6)**.
   - Cliquez sur **OK** pour appliquer les modifications.

![Capture d'écran 2024-07-19 092346](https://github.com/user-attachments/assets/30ae57c9-c3bd-43ef-b9c2-43e9641e9822)

![Capture d'écran 2024-07-19 092610](https://github.com/user-attachments/assets/020b0e21-c8e3-4df4-a794-80d241a1fd51)

### Configuration réseaux DHCP  

![Capture d'écran 2024-07-19 093027](https://github.com/user-attachments/assets/358c4c44-8b1b-400e-9f04-1871780bde19)

![Capture d'écran 2024-07-19 093231](https://github.com/user-attachments/assets/e33ee5fd-c0a9-485b-9d4a-15b14d1e5c2f)

![Capture d'écran 2024-07-19 093326](https://github.com/user-attachments/assets/b88583ce-66e4-4145-8179-362bfc6f9d8f)

![Capture d'écran 2024-07-19 093706](https://github.com/user-attachments/assets/c0dfd53f-7735-47be-aef9-a7ea52ac845d)

![Capture d'écran 2024-07-19 095045](https://github.com/user-attachments/assets/5606d196-1598-401d-afc8-38e396802082)

Q1.5 : 
![Capture d'écran 2024-07-19 095101](https://github.com/user-attachments/assets/a559a09a-fda6-470b-b0b3-65ab7a0bff75)

Q1.6 : 

![Capture d'écran 2024-07-19 101934](https://github.com/user-attachments/assets/c3b47b62-215a-4476-a783-08a0ddbfa658)
Q1.7 : 
Passer à IPv6 n'est pas simplement une réponse à l'épuisement des adresses IPv4. C'est une avancée vers une infrastructure réseau plus sécurisée, efficace et capable de supporter l'énorme croissance des dispositifs connectés et des nouvelles technologies. Bien que la transition nécessite un investissement en termes de mise à jour des équipements et des compétences, les bénéfices à long terme rendent IPv6 essentiel pour l'évolution des réseaux modernes.

Q1.8 : 
Non, le serveur DHCP n'est pas obsolète dans un réseau IPv6. Il continue à jouer un rôle important en gérant les adresses IP et en fournissant des informations de configuration supplémentaires aux clients. En IPv6, le DHCP est connu sous le nom de DHCPv6. 
