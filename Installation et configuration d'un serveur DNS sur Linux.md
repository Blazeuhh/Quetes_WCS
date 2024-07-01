Installation et Configuration de BIND 9 sur Ubuntu
==================================================

1\. Installer le package BIND 9
-------------------------------

Nous devons installer les packages bind9, bind9utils, bind9-doc, et dnsutils pour BIND 9 et les outils associés. Ouvrez votre terminal et exécutez la commande suivante :

Plain textANTLR4BashCC#CSSCoffeeScriptCMakeDartDjangoDockerEJSErlangGitGoGraphQLGroovyHTMLJavaJavaScriptJSONJSXKotlinLaTeXLessLuaMakefileMarkdownMATLABMarkupObjective-CPerlPHPPowerShell.propertiesProtocol BuffersPythonRRubySass (Sass)Sass (Scss)SchemeSQLShellSwiftSVGTSXTypeScriptWebAssemblyYAMLXML`   bashCopy code$ sudo apt update  $ sudo apt install -y bind9 bind9utils bind9-doc dnsutils   `

2\. Configurer BIND (Serveur DNS) sur Ubuntu 24.04 | 22.04
----------------------------------------------------------

Une fois tous les packages installés, nous passerons à la configuration. Tous les fichiers de configuration de BIND se trouvent dans le dossier /etc/bind.

Un fichier de configuration important pour BIND est /etc/bind/named.conf.options, où nous pouvons définir les paramètres suivants :

*   Autoriser les requêtes DNS depuis votre réseau privé.
    
*   Autoriser les requêtes récursives.
    
*   Spécifier le port DNS (53).
    
*   Redirections DNS (les requêtes DNS seront redirigées vers les redirections lorsque votre serveur DNS local ne peut pas résoudre la requête).
    

Selon mes paramètres de réseau privé, j'ai spécifié les paramètres suivants :

Plain textANTLR4BashCC#CSSCoffeeScriptCMakeDartDjangoDockerEJSErlangGitGoGraphQLGroovyHTMLJavaJavaScriptJSONJSXKotlinLaTeXLessLuaMakefileMarkdownMATLABMarkupObjective-CPerlPHPPowerShell.propertiesProtocol BuffersPythonRRubySass (Sass)Sass (Scss)SchemeSQLShellSwiftSVGTSXTypeScriptWebAssemblyYAMLXML`   bashCopy code$ sudo vi /etc/bind/named.conf.options   `

Plain textANTLR4BashCC#CSSCoffeeScriptCMakeDartDjangoDockerEJSErlangGitGoGraphQLGroovyHTMLJavaJavaScriptJSONJSXKotlinLaTeXLessLuaMakefileMarkdownMATLABMarkupObjective-CPerlPHPPowerShell.propertiesProtocol BuffersPythonRRubySass (Sass)Sass (Scss)SchemeSQLShellSwiftSVGTSXTypeScriptWebAssemblyYAMLXML`   confCopy codeacl internal-network {      192.168.0.0/24;  };  options {      directory "/var/cache/bind";      allow-query { localhost; internal-network; };      allow-transfer { localhost; };      forwarders { 8.8.8.8; };      recursion yes;      dnssec-validation auto;      listen-on-v6 { any; };  };   `

Le fichier de configuration suivant est /etc/bind/named.conf.local, où nous définirons les fichiers de zone pour notre domaine. Éditez le fichier et ajoutez les entrées suivantes :

Plain textANTLR4BashCC#CSSCoffeeScriptCMakeDartDjangoDockerEJSErlangGitGoGraphQLGroovyHTMLJavaJavaScriptJSONJSXKotlinLaTeXLessLuaMakefileMarkdownMATLABMarkupObjective-CPerlPHPPowerShell.propertiesProtocol BuffersPythonRRubySass (Sass)Sass (Scss)SchemeSQLShellSwiftSVGTSXTypeScriptWebAssemblyYAMLXML`   bashCopy code$ cd /etc/bind  $ sudo vi named.conf.local   `

Plain textANTLR4BashCC#CSSCoffeeScriptCMakeDartDjangoDockerEJSErlangGitGoGraphQLGroovyHTMLJavaJavaScriptJSONJSXKotlinLaTeXLessLuaMakefileMarkdownMATLABMarkupObjective-CPerlPHPPowerShell.propertiesProtocol BuffersPythonRRubySass (Sass)Sass (Scss)SchemeSQLShellSwiftSVGTSXTypeScriptWebAssemblyYAMLXML`   confCopy codezone "linuxtechi.local" IN {      type master;      file "/etc/bind/forward.linuxtechi.local";      allow-update { none; };  };  zone "0.168.192.in-addr.arpa" IN {      type master;      file "/etc/bind/reverse.linuxtechi.local";      allow-update { none; };  };   `

Enregistrez le fichier et quittez. Ensuite, nous créerons les fichiers de zone de recherche directe et inverse mentionnés.

Pour créer le fichier de zone de recherche directe :

Plain textANTLR4BashCC#CSSCoffeeScriptCMakeDartDjangoDockerEJSErlangGitGoGraphQLGroovyHTMLJavaJavaScriptJSONJSXKotlinLaTeXLessLuaMakefileMarkdownMATLABMarkupObjective-CPerlPHPPowerShell.propertiesProtocol BuffersPythonRRubySass (Sass)Sass (Scss)SchemeSQLShellSwiftSVGTSXTypeScriptWebAssemblyYAMLXML`   bashCopy code$ cd /etc/bind  $ sudo cp db.local forward.linuxtechi.local  $ sudo vi forward.linuxtechi.local   `

Plain textANTLR4BashCC#CSSCoffeeScriptCMakeDartDjangoDockerEJSErlangGitGoGraphQLGroovyHTMLJavaJavaScriptJSONJSXKotlinLaTeXLessLuaMakefileMarkdownMATLABMarkupObjective-CPerlPHPPowerShell.propertiesProtocol BuffersPythonRRubySass (Sass)Sass (Scss)SchemeSQLShellSwiftSVGTSXTypeScriptWebAssemblyYAMLXML`   zoneCopy code$TTL 604800  @ IN SOA primary.linuxtechi.local. root.primary.linuxtechi.local. (      2022072651 ; Serial      3600 ; Refresh      1800 ; Retry      604800 ; Expire      604600 ; Negative Cache TTL  )  ; Informations sur le serveur de noms  @ IN NS primary.linuxtechi.local.  ; Adresse IP de votre serveur DNS  primary IN A 192.168.0.40  ; Enregistrement MX (Mail Exchanger)  linuxtechi.local. IN MX 10 mail.linuxtechi.local.  ; Enregistrement A pour les noms d'hôtes  www IN A 192.168.0.50  mail IN A 192.168.0.60  ; Enregistrement CNAME  ftp IN CNAME www.linuxtechi.local.   `

Pour créer le fichier de zone de recherche inverse :

Plain textANTLR4BashCC#CSSCoffeeScriptCMakeDartDjangoDockerEJSErlangGitGoGraphQLGroovyHTMLJavaJavaScriptJSONJSXKotlinLaTeXLessLuaMakefileMarkdownMATLABMarkupObjective-CPerlPHPPowerShell.propertiesProtocol BuffersPythonRRubySass (Sass)Sass (Scss)SchemeSQLShellSwiftSVGTSXTypeScriptWebAssemblyYAMLXML`   bashCopy code$ sudo cp db.127 reverse.linuxtechi.local  $ sudo vi /etc/bind/reverse.linuxtechi.local   `

Plain textANTLR4BashCC#CSSCoffeeScriptCMakeDartDjangoDockerEJSErlangGitGoGraphQLGroovyHTMLJavaJavaScriptJSONJSXKotlinLaTeXLessLuaMakefileMarkdownMATLABMarkupObjective-CPerlPHPPowerShell.propertiesProtocol BuffersPythonRRubySass (Sass)Sass (Scss)SchemeSQLShellSwiftSVGTSXTypeScriptWebAssemblyYAMLXML`   zoneCopy code$TTL 86400  @ IN SOA linuxtechi.local. root.linuxtechi.local. (      2022072752 ; Serial      3600 ; Refresh      1800 ; Retry      604800 ; Expire      86400 ; Minimum TTL  )  ; Informations sur le serveur de noms  @ IN NS primary.linuxtechi.local.  primary IN A 192.168.0.40  ; Recherche inverse pour votre serveur DNS  40 IN PTR primary.linuxtechi.local.  ; Enregistrement PTR adresse IP à nom d'hôte  50 IN PTR www.linuxtechi.local.  60 IN PTR mail.linuxtechi.local.   `

Enregistrez le fichier et quittez.

Mettez à jour le paramètre suivant dans le fichier /etc/default/named pour que le service DNS commence à écouter sur IPv4 :

Plain textANTLR4BashCC#CSSCoffeeScriptCMakeDartDjangoDockerEJSErlangGitGoGraphQLGroovyHTMLJavaJavaScriptJSONJSXKotlinLaTeXLessLuaMakefileMarkdownMATLABMarkupObjective-CPerlPHPPowerShell.propertiesProtocol BuffersPythonRRubySass (Sass)Sass (Scss)SchemeSQLShellSwiftSVGTSXTypeScriptWebAssemblyYAMLXML`   confCopy codeOPTIONS="-u bind -4"   `

Ensuite, démarrez et activez le service BIND pour appliquer les modifications :

Plain textANTLR4BashCC#CSSCoffeeScriptCMakeDartDjangoDockerEJSErlangGitGoGraphQLGroovyHTMLJavaJavaScriptJSONJSXKotlinLaTeXLessLuaMakefileMarkdownMATLABMarkupObjective-CPerlPHPPowerShell.propertiesProtocol BuffersPythonRRubySass (Sass)Sass (Scss)SchemeSQLShellSwiftSVGTSXTypeScriptWebAssemblyYAMLXML`   bashCopy code$ sudo systemctl start named  $ sudo systemctl enable named   `

Vérifiez le statut du service BIND :

Plain textANTLR4BashCC#CSSCoffeeScriptCMakeDartDjangoDockerEJSErlangGitGoGraphQLGroovyHTMLJavaJavaScriptJSONJSXKotlinLaTeXLessLuaMakefileMarkdownMATLABMarkupObjective-CPerlPHPPowerShell.propertiesProtocol BuffersPythonRRubySass (Sass)Sass (Scss)SchemeSQLShellSwiftSVGTSXTypeScriptWebAssemblyYAMLXML`   bashCopy code$ sudo systemctl status named   `

**Note** : Si le pare-feu de l'OS fonctionne sur votre serveur BIND, exécutez la commande suivante pour autoriser le port 53 :

Plain textANTLR4BashCC#CSSCoffeeScriptCMakeDartDjangoDockerEJSErlangGitGoGraphQLGroovyHTMLJavaJavaScriptJSONJSXKotlinLaTeXLessLuaMakefileMarkdownMATLABMarkupObjective-CPerlPHPPowerShell.propertiesProtocol BuffersPythonRRubySass (Sass)Sass (Scss)SchemeSQLShellSwiftSVGTSXTypeScriptWebAssemblyYAMLXML`   bashCopy code$ sudo ufw allow 53   `

3\. Validation de la syntaxe des fichiers de configuration et des fichiers de zone de BIND
------------------------------------------------------------------------------------------

Pour vérifier la syntaxe de votre fichier de configuration BIND (named.conf.local), utilisez la commande named-checkconf :

Plain textANTLR4BashCC#CSSCoffeeScriptCMakeDartDjangoDockerEJSErlangGitGoGraphQLGroovyHTMLJavaJavaScriptJSONJSXKotlinLaTeXLessLuaMakefileMarkdownMATLABMarkupObjective-CPerlPHPPowerShell.propertiesProtocol BuffersPythonRRubySass (Sass)Sass (Scss)SchemeSQLShellSwiftSVGTSXTypeScriptWebAssemblyYAMLXML`   bashCopy code$ sudo named-checkconf /etc/bind/named.conf.local   `

Si aucune erreur de syntaxe n'est présente, la commande retournera à l'invite sans afficher d'erreurs.

Pour vérifier la syntaxe de vos fichiers de zone de recherche directe et inverse, utilisez la commande named-checkzone :

Plain textANTLR4BashCC#CSSCoffeeScriptCMakeDartDjangoDockerEJSErlangGitGoGraphQLGroovyHTMLJavaJavaScriptJSONJSXKotlinLaTeXLessLuaMakefileMarkdownMATLABMarkupObjective-CPerlPHPPowerShell.propertiesProtocol BuffersPythonRRubySass (Sass)Sass (Scss)SchemeSQLShellSwiftSVGTSXTypeScriptWebAssemblyYAMLXML`   bashCopy code$ sudo named-checkzone linuxtechi.local /etc/bind/forward.linuxtechi.local  zone linuxtechi.local/IN: loaded serial 2022072651  OK   `

Plain textANTLR4BashCC#CSSCoffeeScriptCMakeDartDjangoDockerEJSErlangGitGoGraphQLGroovyHTMLJavaJavaScriptJSONJSXKotlinLaTeXLessLuaMakefileMarkdownMATLABMarkupObjective-CPerlPHPPowerShell.propertiesProtocol BuffersPythonRRubySass (Sass)Sass (Scss)SchemeSQLShellSwiftSVGTSXTypeScriptWebAssemblyYAMLXML`   bashCopy code$ sudo named-checkzone linuxtechi.local /etc/bind/reverse.linuxtechi.local  zone linuxtechi.local/IN: loaded serial 2022072752  OK   `

4\. Tester le serveur DNS avec dig & nslookup
---------------------------------------------

Pour tester notre serveur DNS BIND 9, nous utiliserons une autre machine Ubuntu et changerons son DNS pour pointer vers notre serveur DNS. Pour changer le serveur DNS, ouvrez /etc/resolv.conf et faites l'entrée DNS suivante :

Plain textANTLR4BashCC#CSSCoffeeScriptCMakeDartDjangoDockerEJSErlangGitGoGraphQLGroovyHTMLJavaJavaScriptJSONJSXKotlinLaTeXLessLuaMakefileMarkdownMATLABMarkupObjective-CPerlPHPPowerShell.propertiesProtocol BuffersPythonRRubySass (Sass)Sass (Scss)SchemeSQLShellSwiftSVGTSXTypeScriptWebAssemblyYAMLXML`   bashCopy code$ sudo vi /etc/resolv.conf   `

Plain textANTLR4BashCC#CSSCoffeeScriptCMakeDartDjangoDockerEJSErlangGitGoGraphQLGroovyHTMLJavaJavaScriptJSONJSXKotlinLaTeXLessLuaMakefileMarkdownMATLABMarkupObjective-CPerlPHPPowerShell.propertiesProtocol BuffersPythonRRubySass (Sass)Sass (Scss)SchemeSQLShellSwiftSVGTSXTypeScriptWebAssemblyYAMLXML`   confCopy codesearch linuxtechi.local  nameserver 192.168.0.40   `

Enregistrez le fichier et quittez. Maintenant, notre client est prêt avec le DNS pointant vers notre serveur. Nous utiliserons ensuite l'outil CLI dig pour vérifier les informations DNS. Exécutez la commande suivante depuis le terminal :

Plain textANTLR4BashCC#CSSCoffeeScriptCMakeDartDjangoDockerEJSErlangGitGoGraphQLGroovyHTMLJavaJavaScriptJSONJSXKotlinLaTeXLessLuaMakefileMarkdownMATLABMarkupObjective-CPerlPHPPowerShell.propertiesProtocol BuffersPythonRRubySass (Sass)Sass (Scss)SchemeSQLShellSwiftSVGTSXTypeScriptWebAssemblyYAMLXML`   bashCopy code$ dig primary.linuxtechi.local   `

Vous devriez obtenir une sortie similaire à celle-ci, indiquant que notre DNS fonctionne correctement. Pour une recherche inverse (PTR) :

Plain textANTLR4BashCC#CSSCoffeeScriptCMakeDartDjangoDockerEJSErlangGitGoGraphQLGroovyHTMLJavaJavaScriptJSONJSXKotlinLaTeXLessLuaMakefileMarkdownMATLABMarkupObjective-CPerlPHPPowerShell.propertiesProtocol BuffersPythonRRubySass (Sass)Sass (Scss)SchemeSQLShellSwiftSVGTSXTypeScriptWebAssemblyYAMLXML`   bashCopy code$ dig -x 192.168.0.40   `

La sortie de la commande devrait ressembler à ceci :

Plain textANTLR4BashCC#CSSCoffeeScriptCMakeDartDjangoDockerEJSErlangGitGoGraphQLGroovyHTMLJavaJavaScriptJSONJSXKotlinLaTeXLessLuaMakefileMarkdownMATLABMarkupObjective-CPerlPHPPowerShell.propertiesProtocol BuffersPythonRRubySass (Sass)Sass (Scss)SchemeSQLShellSwiftSVGTSXTypeScriptWebAssemblyYAMLXML`   plaintextCopy code;; ANSWER SECTION:  40.0.168.192.in-addr.arpa. 86400 IN PTR primary.linuxtechi.local.   `