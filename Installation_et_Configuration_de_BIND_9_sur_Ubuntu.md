# Installation et Configuration de BIND 9 sur Ubuntu

# Installation et Configuration de BIND 9 sur Ubuntu

## 1. Installer le package BIND 9

Nous devons installer les packages bind9, bind9utils, bind9-doc, et dnsutils pour BIND 9 et les outils associés. Ouvrez votre terminal et exécutez la commande suivante :

bash

Copy code

$ sudo apt update

$ sudo apt install -y bind9 bind9utils bind9-doc dnsutils

## 2. Configurer BIND (Serveur DNS) sur Ubuntu 24.04 | 22.04

Une fois tous les packages installés, nous passerons à la configuration. Tous les fichiers de configuration de BIND se trouvent dans le dossier /etc/bind.

Un fichier de configuration important pour BIND est /etc/bind/named.conf.options, où nous pouvons définir les paramètres suivants :

- Autoriser les requêtes DNS depuis votre réseau privé.
- Autoriser les requêtes récursives.
- Spécifier le port DNS (53).
- Redirections DNS (les requêtes DNS seront redirigées vers les redirections lorsque votre serveur DNS local ne peut pas résoudre la requête).

Selon mes paramètres de réseau privé, j'ai spécifié les paramètres suivants :

bash

Copy code

$ sudo vi /etc/bind/named.conf.options

conf

Copy code

acl internal-network {

    192.168.0.0/24;

};



options {

    directory "/var/cache/bind";

    allow-query { localhost; internal-network; };

    allow-transfer { localhost; };

    forwarders { 8.8.8.8; };

    recursion yes;

    dnssec-validation auto;

    listen-on-v6 { any; };

};

Le fichier de configuration suivant est /etc/bind/named.conf.local, où nous définirons les fichiers de zone pour notre domaine. Éditez le fichier et ajoutez les entrées suivantes :

bash

Copy code

$ cd /etc/bind

$ sudo vi named.conf.local

conf

Copy code

zone "wilders.lan" IN {

    type master;

    file "/etc/bind/forward.wilders.lan";

    allow-update { none; };

};



zone "0.168.192.in-addr.arpa" IN {

    type master;

    file "/etc/bind/reverse.wilders.lan";

    allow-update { none; };

};

Enregistrez le fichier et quittez. Ensuite, nous créerons les fichiers de zone de recherche directe et inverse mentionnés.

Pour créer le fichier de zone de recherche directe :

bash

Copy code

$ cd /etc/bind

$ sudo cp db.local forward.wilders.lan

$ sudo vi forward.wilders.lan

zone

Copy code

$TTL 604800

@ IN SOA primary.wilders.lan. root.primary.wilders.lan. (

    2022072651 ; Serial

    3600 ; Refresh

    1800 ; Retry

    604800 ; Expire

    604600 ; Negative Cache TTL

)

; Informations sur le serveur de noms

@ IN NS primary.wilders.lan.



; Adresse IP de votre serveur DNS

primary IN A 192.168.0.40



; Enregistrement MX (Mail Exchanger)

wilders.lan. IN MX 10 mail.wilders.lan.



; Enregistrement A pour les noms d'hôtes

www IN A 192.168.0.50

mail IN A 192.168.0.60



; Enregistrement CNAME

ftp IN CNAME www\.wilders.lan.

Pour créer le fichier de zone de recherche inverse :

bash

Copy code

$ sudo cp db.127 reverse.wilders.lan

$ sudo vi /etc/bind/reverse.wilders.lan

zone

Copy code

$TTL 86400

@ IN SOA wilders.lan. root.wilders.lan. (

    2022072752 ; Serial

    3600 ; Refresh

    1800 ; Retry

    604800 ; Expire

    86400 ; Minimum TTL

)

; Informations sur le serveur de noms

@ IN NS primary.wilders.lan.

primary IN A 192.168.0.40



; Recherche inverse pour votre serveur DNS

40 IN PTR primary.wilders.lan.

; Enregistrement PTR adresse IP à nom d'hôte

50 IN PTR www\.wilders.lan.

60 IN PTR mail.wilders.lan.

Enregistrez le fichier et quittez.

Mettez à jour le paramètre suivant dans le fichier /etc/default/named pour que le service DNS commence à écouter sur IPv4 :

conf

Copy code

OPTIONS="-u bind -4"

Ensuite, démarrez et activez le service BIND pour appliquer les modifications :

bash

Copy code

$ sudo systemctl start named

$ sudo systemctl enable named

Vérifiez le statut du service BIND :

bash

Copy code

$ sudo systemctl status named

**Note** : Si le pare-feu de l'OS fonctionne sur votre serveur BIND, exécutez la commande suivante pour autoriser le port 53 :

bash

Copy code

$ sudo ufw allow 53

## 3. Validation de la syntaxe des fichiers de configuration et des fichiers de zone de BIND

Pour vérifier la syntaxe de votre fichier de configuration BIND (named.conf.local), utilisez la commande named-checkconf :

bash

Copy code

$ sudo named-checkconf /etc/bind/named.conf.local

Si aucune erreur de syntaxe n'est présente, la commande retournera à l'invite sans afficher d'erreurs.

Pour vérifier la syntaxe de vos fichiers de zone de recherche directe et inverse, utilisez la commande named-checkzone :

bash

Copy code

$ sudo named-checkzone wilders.lan /etc/bind/forward.wilders.lan

zone wilders.lan/IN: loaded serial 2022072651

OK

bash

Copy code

$ sudo named-checkzone wilders.lan /etc/bind/reverse.wilders.lan

zone wilders.lan/IN: loaded serial 2022072752

OK

## 4. Tester le serveur DNS avec dig & nslookup

Pour tester notre serveur DNS BIND 9, nous utiliserons une autre machine Ubuntu et changerons son DNS pour pointer vers notre serveur DNS. Pour changer le serveur DNS, ouvrez /etc/resolv.conf et faites l'entrée DNS suivante :

bash

Copy code

$ sudo vi /etc/resolv.conf

conf

Copy code

search wilders.lan

nameserver 192.168.0.40

Enregistrez le fichier et quittez. Maintenant, notre client est prêt avec le DNS pointant vers notre serveur. Nous utiliserons ensuite l'outil CLI dig pour vérifier les informations DNS. Exécutez la commande suivante depuis le terminal :

bash

Copy code

$ dig primary.wilders.lan

Vous devriez obtenir une sortie similaire à celle-ci, indiquant que notre DNS fonctionne correctement. Pour une recherche inverse (PTR) :

bash

Copy code

$ dig -x 192.168.0.40

La sortie de la commande devrait ressembler à ceci :

plaintext

Copy code

;; ANSWER SECTION:

40.0.168.192.in-addr.arpa. 86400 IN PTR primary.wilders.lan.

-
