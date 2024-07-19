### Q.3.1 Quel est le matériel réseau A ?

C'est un switch, il permet de connecter les ordinateurs entre-eux mais en local seulement.

### Q.3.2 Quel est le matériel réseau B ?

C'est un routeur, il permet de relier les réseaux d'ordinateurs entre-eux , ici le réseau `10.10.0.0/16` va pouvoir communiquer avec le cloud grâce aux interventions des 2 routeurs.

### Q.3.3 Que signifie `f0/0` et `g1/0` sur l’élément B ?

Ce sont des interfaces réseaux, des ports d'entrées et de sorties pour communiquer entre les différents matériels réseaux.

### Q.3.4 Pour l'ordinateur PC2, que représente `/16` dans son adresse IP ?

Dans le contexte des adresses IP, le suffixe `/16` est une notation en CIDR (*Classless Inter-Domain Routing*) qui spécifie la longueur du préfixe du réseau. Plus précisément, `/16` indique que les 16 premiers bits de l'adresse IP sont utilisés pour définir la partie réseau de l'adresse, tandis que les bits restants sont utilisés pour les adresses des hôtes au sein de ce réseau.

### Q.3.5 Pour ce même ordinateur, que représente l'adresse `10.10.255.254` ?

Elle est interprété comme une adresse de diffusion ou de passerelle dans certains contextes, mais en général, elle est simplement une adresse d'hôte valide.
