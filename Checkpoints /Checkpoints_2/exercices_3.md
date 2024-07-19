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

### Q.3.15 Quels ont été les rôles des matériels A et B dans cette communication ?

### Fichier 2 :

### Q.3.16 Dans cette trame, qui initialise la communication ? Donne l'adresse IP ainsi que le nom du matériel.

### Q.3.17 Quel est le protocole encapsulé ? Quel est son rôle ?

### Q.3.18 Est-ce que cette communication a réussi ? Si oui, indique entre quels matériel, si non indique pourquoi cela n'a pas fonctionné.

### Q.3.19 Explique la ligne du paquet N° 2

### Q.3.20 Quels ont été les rôles des matériels A et B ?

### Fichier 3 :

### Q.3.21 Dans cette trame, donne les noms et les adresses IP des matériels sources et destination.

### Q.3.22 Quelles sont les adresses mac source et destination ? Qu'en déduis-tu ?

### Q.3.23 A quel emplacement du réseau a été enregistré cette communication ?
