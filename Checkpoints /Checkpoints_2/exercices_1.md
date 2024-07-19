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

### Q.1.2 Ping avec les noms des machines 

Si les pings sont réussis, cela confirme que la résolution de noms fonctionne correctement.

![Capture d'écran 2024-07-19 092610](https://github.com/user-attachments/assets/020b0e21-c8e3-4df4-a794-80d241a1fd51)

### Q.1.3 Désactivation du protocole IPv6 et vérification

   - Faites un clic droit sur l'icône réseau dans la barre des tâches et sélectionnez **Ouvrir les paramètres de réseau et Internet**.
   - Dans la fenêtre qui s'ouvre, cliquez sur **Centre Réseau et Partage**.
   - Cliquez sur **Modifier les paramètres de l’adaptateur** .
   - Décochez la case à côté de **Protocole Internet version 6 (TCP/IPv6)**.
   - Cliquez sur **OK** pour appliquer les modifications.

![Capture d'écran 2024-07-19 092346](https://github.com/user-attachments/assets/30ae57c9-c3bd-43ef-b9c2-43e9641e9822)

Nous retestions comme demandé si il y a une anomalie avec le ping , il n'y a pas de soucis apparent :

![Capture d'écran 2024-07-19 092610](https://github.com/user-attachments/assets/020b0e21-c8e3-4df4-a794-80d241a1fd51)

### Q.1.4 Configuration DHCP

- Ici on peut voir que le serveur a déjà le DNS et le DHCP protocoles d'installés donc je vais passer à la configuration : 

![Capture d'écran 2024-07-19 093027](https://github.com/user-attachments/assets/358c4c44-8b1b-400e-9f04-1871780bde19)

- Allez dans **Outils** puis cliquez sur DHCP.

![Capture d'écran 2024-07-19 093231](https://github.com/user-attachments/assets/e33ee5fd-c0a9-485b-9d4a-15b14d1e5c2f)

- Déroulez l'arborescence puis faites clique droit sur **IPV4** afin de **Créer une nouvelle étendue**

![Capture d'écran 2024-07-19 093326](https://github.com/user-attachments/assets/b88583ce-66e4-4145-8179-362bfc6f9d8f)

- Donnez des plages d'adresses que vous voulez mettre pour attribuer les adresses IP souhaités à vos machines ( ici cela sera 172.16.10.11 jusqu'à 172.16.10.20 ) 

![Capture d'écran 2024-07-19 093706](https://github.com/user-attachments/assets/c0dfd53f-7735-47be-aef9-a7ea52ac845d)

- Ici nous pouvons voir que notre étendue à bien été créer, d'ailleurs une autre étendue été déjà présente ?

![Capture d'écran 2024-07-19 095045](https://github.com/user-attachments/assets/5606d196-1598-401d-afc8-38e396802082)

### Q.1.5 Pourquoi le client ne récupère pas la 1ère adresse disponible ?

- Les serveurs DHCP utilisent souvent des mécanismes pour éviter l'attribution de la première adresse d'une plage pour éviter les conflits avec des adresses statiques ou pour réserver certaines adresses. Mais dans ce cas précis nous pouvons voir qu'il y a une étendue déjà de créer. Si l'on clique dessus et qu'on va dans les **Attributions d'adresses** , on constate qu'il y a des restrictions d'adresses. On peut voir ceci :
  
![Capture d'écran 2024-07-19 095101](https://github.com/user-attachments/assets/a559a09a-fda6-470b-b0b3-65ab7a0bff75)

Cela signifie que les plages `172.16.10.1` jusqu'à `172.16.10.19` et `172.16.10.241` jusqu'à `172.16.10.254` ne peuvent pas être prise. C'est pour cela que l'adresse Ip que notre client a prise est celle-ci :

![Capture d'écran 2024-07-19 133126](https://github.com/user-attachments/assets/83f933da-7216-48af-b0da-1ebdfe4e5646)

### Q.1.6 Configuration DHCP pour une adresse IP fixe

Pour mettre l'adresse IP  `172.16.10.15` au client , il va falloir tout d'abord supprimer la scope qui nous empêche de créer `172.16.10.1` jusqu'à `172.16.10.19` 
Ensuite il va falloir créer une réservation :
 - Allez dans le menu **DHCP**, **IPV4**, faites un clique droit sur **Réservations** puis **New-réservations**
 - Complétez les informations comme ci-dessous :
 
 ![Capture d'écran 2024-07-19 101934](https://github.com/user-attachments/assets/c3b47b62-215a-4476-a783-08a0ddbfa658)

 - Pour finir faites "Ajouter"
 - 
### Q.1.7 Pourquoi passez ce réseau en IPV6?

- Passer à IPv6 n'est pas simplement une réponse à l'épuisement des adresses IPv4. C'est une avancée vers une infrastructure réseau plus sécurisée, efficace et capable de supporter l'énorme croissance des dispositifs connectés et des nouvelles technologies. Bien que la transition nécessite un investissement en termes de mise à jour des équipements et des compétences, les bénéfices à long terme rendent IPv6 essentiel pour l'évolution des réseaux modernes.

### Q.1.8 Serveur DHCP obsolète?

- Non, le serveur DHCP n'est pas obsolète dans un réseau IPv6. Il continue à jouer un rôle important en gérant les adresses IP et en fournissant des informations de configuration supplémentaires aux clients. En IPv6, le DHCP est connu sous le nom de DHCPv6. 


