## Vérification d'une infrastructure réseau : 

### Q.3.1 Quel est le matériel réseau A ?

- C'est un switch, il permet de connecter les ordinateurs entre-eux mais en local seulement.

### Q.3.2 Quel est le matériel réseau B ?

- C'est un routeur, il permet de relier les réseaux d'ordinateurs entre-eux , ici le réseau `10.10.0.0/16` va pouvoir communiquer avec le cloud grâce aux interventions des 2 routeurs.

### Q.3.3 Que signifie `f0/0` et `g1/0` sur l’élément B ?

- Ce sont des interfaces réseaux, des ports d'entrées et de sorties pour communiquer entre les différents matériels réseaux.

### Q.3.4 Pour l'ordinateur PC2, que représente `/16` dans son adresse IP ?

- Dans le contexte des adresses IP, le suffixe `/16` est une notation en CIDR (*Classless Inter-Domain Routing*) qui spécifie la longueur du préfixe du réseau. Plus précisément, `/16` indique que les 16 premiers bits de l'adresse IP sont utilisés pour définir la partie réseau de l'adresse, tandis que les bits restants sont utilisés pour les adresses des hôtes au sein de ce réseau.

### Q.3.5 Pour ce même ordinateur, que représente l'adresse 10.10.255.254 ?

- Elle est souvent utilisée comme adresse de diffusion ou de passerelle dans certains contextes, mais en général, elle est simplement une adresse d'hôte valide.

### Q.3.6 L'adresse de réseau, la première adresse disponible, la dernière adresse disponible, l'adresse de diffusion du PC1,2 et 5 

- **PC1 (`10.10.4.1/16`):**
  - Adresse de réseau : `10.10.0.0`
  - Première adresse disponible : `10.10.0.1`
  - Dernière adresse disponible : `10.10.255.254`
  - Adresse de diffusion : `10.10.255.255`

- **PC2 (`10.11.80.2/16`):**
  - Adresse de réseau : `10.11.0.0`
  - Première adresse disponible : `10.11.0.1`
  - Dernière adresse disponible : `10.11.255.254`
  - Adresse de diffusion : `10.11.255.255`

- **PC5 (`10.10.4.7/15`):**
  - Adresse de réseau : `10.10.0.0`
  - Première adresse disponible : `10.10.0.1`
  - Dernière adresse disponible : `10.11.255.254`
  - Adresse de diffusion : `10.11.255.255`
 
### Q.3.7 Indique parmi tous les ordinateurs du schéma, lesquels vont pouvoir communiquer entre-eux.

- PC1, PC3, PC4, et PC5 peuvent tous communiquer directement entre eux.
- PC2 peut communiquer directement uniquement avec R2 g2/0 et B g1/0 (si le routage est configuré pour traverser les sous-réseaux).

### Q.3.8 Indique parmi tous les ordinateurs du schéma, lesquels vont pouvoir communiquer entre-eux.

- Tous les ordinateurs et équipements de réseau listés auront besoin de routage pour atteindre le réseau 172.16.5.0/24, car ils sont tous situés dans des sous-réseaux différents. Cela signifie qu'aucun des appareils ne peut atteindre directement 172.16.5.0/24 sans passer par un routeur configuré pour acheminer le trafic vers ce réseau.

### Q.3.9 Quel incidence y-a-t'il pour les ordinateurs PC2 et PC3 si on interverti leur ports de connexion sur le matériel A ?

- L'inversion des ports n'affectera généralement pas la capacité des PC2 et PC3 à communiquer avec leurs propres sous-réseaux, tant que les ports sont correctement configurés dans le même VLAN. 

### Q.3.10 Quel incidence y-a-t'il pour les ordinateurs PC2 et PC3 si on interverti leur ports de connexion sur le matériel A ?

- Pour configurer les adresses IP des ordinateurs de manière dynamique, il est nécessaire de mettre en place un serveur DHCP (Dynamic Host Configuration Protocol) dans le réseau. Le DHCP est un protocole qui permet aux appareils d'obtenir automatiquement des adresses IP et d'autres paramètres de configuration réseau sans avoir à les configurer manuellement.

## Analyse de trames Wireshark : 

### Fichier 1 :

### Q.3.11 Sur le paquet N°5, quelle est l'adresse mac du matériel qui initialise la communication ? Déduis-en le nom du matériel.

- **Adresse MAC Source (Initie la Communication) :** `00:50:79:66:68:00`

- **PC1 :** `00:50:79:66:68:00`, Adresse IP : `10.10.4.1`
- **PC4 :** `00:50:79:66:68:03`, Adresse IP : `10.10.4.2`


### Q.3.12 Est-ce que la communication enregistrée dans cette capture a réussi ? Si oui, indique entre quels matériel, si non indique pourquoi cela n'a pas fonctionné.

La trame initiale montre une requête de ping de PC1 vers PC4. Si la trame de réponse (Frame 6) est présente, cela confirme que la communication a réussi entre PC1 et PC4.

### Q.3.13 Dans cette capture, à quel matériel correspond le request et le reply ? Donne le nom, l'adresse IP, et l'adresse mac de chaque materiel.

Request  :
Matériel : PC1
Adresse IP Source : 10.10.4.1
Adresse MAC Source : 00:50:79:66:68:00
Reply  :
Matériel : PC4
Adresse IP Destination : 10.10.4.1
Adresse MAC Destination : 00:50:79:66:68:00

### Q.3.14 Dans le paquet N°2, quel est le protocole encapsulé ? Quel est son rôle ?

Protocole Encapsulé : ARP (Address Resolution Protocol)
Rôle du Protocole ARP
ARP (Address Resolution Protocol) est un protocole utilisé pour mapper des adresses IP (Internet Protocol) à des adresses MAC (Media Access Control) dans un réseau local. Voici son rôle en détail :

### Q.3.15 Quels ont été les rôles des matériels A et B dans cette communication ?

Matériel A (Switch) :

Responsable de la commutation des trames ARP et ICMP au sein du réseau local.
Assure que les trames sont dirigées vers les ports appropriés en fonction des adresses MAC.
Matériel B (Routeur) :

Serait responsable du routage des paquets entre différents réseaux si la communication nécessitait un accès à un autre sous-réseau.
Non impliqué dans les communications ARP et ICMP observées qui se situent entièrement dans le même réseau local.

### Fichier 2 :

### Q.3.16 - Dans cette trame, qui initialise la communication ? Donne l'adresse IP ainsi que le nom du matériel.

La communication initiale est établie dans la trame précédente, c'est-à-dire la trame n°5. La trame n°6 est une réponse à la requête initiée dans la trame n°5.

**Trame n°5 :** 
- **Source MAC:** 00:50:79:66:68:00
- **Source IP:** 10.10.4.1
- **Destination MAC:** 00:50:79:66:68:03
- **Destination IP:** 10.10.4.2

Le matériel qui initialise la communication dans la trame n°5 est donc **PC1** avec l'adresse IP **10.10.4.1**.

### Q.3.17 - Quel est le protocole encapsulé ? Quel est son rôle ?

**Protocole Encapsulé :** ICMP (Internet Control Message Protocol)

**Rôle du Protocole ICMP :**
- Le protocole ICMP est utilisé pour envoyer des messages de contrôle et des informations d'erreur dans un réseau IP. 
- Les types courants de messages ICMP incluent les requêtes et réponses Echo (utilisées par la commande Ping) pour tester la connectivité entre les dispositifs, ainsi que les messages de destination inaccessible, de redirection, et d'écho-réponse.
- Dans cette trame spécifique (trame n°6), le message ICMP est un **Echo (ping) reply** (Type 0), qui est utilisé pour répondre à une requête Echo (ping) request (Type 8). Cela indique que le dispositif de destination a reçu et traité la requête ping.

### Q.3.18 - Est-ce que cette communication a réussi ? Si oui, indique entre quels matériel, si non indique pourquoi cela n'a pas fonctionné.

**Oui, la communication a réussi.**

#### Détails de la Communication :

- **Requête Ping (trame n°5) :**
  - **Source :** PC1 (10.10.4.1, MAC: 00:50:79:66:68:00)
  - **Destination :** PC4 (10.10.4.2, MAC: 00:50:79:66:68:03)

- **Réponse Ping (trame n°6) :**
  - **Source :** PC4 (10.10.4.2, MAC: 00:50:79:66:68:03)
  - **Destination :** PC1 (10.10.4.1, MAC: 00:50:79:66:68:00)

Le succès de la communication est confirmé par la présence de la réponse ICMP Echo Reply dans la trame n°6, qui répond à la requête Echo Request de la trame n°5. Cela signifie que PC4 a correctement reçu et traité la requête ICMP de PC1 et a renvoyé une réponse.

### Q.3.19 - Explique la ligne du paquet N° 2


1. **Frame 2: 70 bytes on wire (560 bits), 70 bytes captured (560 bits)**

2. **Ethernet II, Src: ca:01:da:d2:00:08, Dst: 00:50:79:66:68:02**
 
3. **Destination: 00:50:79:66:68:02**

4. **Source: ca:01:da:d2:00:08**

5. **Type: IPv4 (0x0800)**

6. **Internet Control Message Protocol**
   

### Q.3.20 - Quels ont été les rôles des matériels A et B ?

Switch A : Responsable de la commutation des trames Ethernet au sein du réseau local, assurant que les trames arrivent aux bons dispositifs en fonction des adresses MAC.
Routeur B : Responsable de l'envoi et du routage des paquets IP entre différents réseaux, en prenant des décisions de routage basées sur les adresses IP source et destination.


### Fichier 3 :

### Q.3.21 - Dans cette trame, donne les noms et les adresses IP des matériels sources et destination

- **Source:**
  - **Nom du Matériel:** PC4
  - **Adresse IP:** 10.10.4.2

- **Destination:**
  - **Nom du Matériel:** C'est une adresse IP appartenant au réseau 172.16.5.0/24. Il s'agit donc potentiellement d'un dispositif sur ce réseau. Sans information supplémentaire, le nom du matériel ne peut être déterminé précisément.
  - **Adresse IP:** 172.16.5.253

### Q.3.22 - Quelles sont les adresses MAC source et destination ? Qu'en déduis-tu ?

- **Adresse MAC Source:**
  - Dans la trame, l'adresse MAC source n'est pas explicitement mentionnée dans la ligne No. 1 du tableau. Cependant, si on se réfère aux informations de la trame précédente (et en supposant une continuité), l'adresse MAC source pourrait être `00:50:79:66:68:03` (adresse MAC de PC4).

- **Adresse MAC Destination:**
  - De même, l'adresse MAC de destination n'est pas explicitement mentionnée dans la ligne No. 1 du tableau. Si l'on suppose que le routeur est impliqué dans le routage des paquets entre les réseaux, l'adresse MAC de destination pourrait être celle du routeur, par exemple `ca:01:da:d2:00:08` (adresse MAC du routeur B).


### Q.3.23 - À quel emplacement du réseau a été enregistrée cette communication ?

La communication ICMP entre **10.10.4.2** et **172.16.5.253** indique un enregistrement potentiellement au niveau du routeur (routeur B), puisque le paquet doit passer par un routeur pour atteindre un réseau différent. Cela signifie que l'enregistrement de la trame a probablement eu lieu au point où les deux réseaux (10.10.0.0/16 et 172.16.5.0/24) sont connectés, c'est-à-dire au niveau du routeur.
