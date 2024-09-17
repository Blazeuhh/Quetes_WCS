
### 1. Installer Asterisk

1. **Mettre à jour les paquets :**

   ```bash
   sudo apt update
   sudo apt upgrade
   ```

2. **Installer les dépendances nécessaires :**

   ```bash
   sudo apt install build-essential subversion libxml2-dev libncurses5-dev libnewt-dev libsqlite3-dev uuid-dev
   ```

3. **Télécharger et installer Asterisk :**

```bash
cd /usr/src
sudo git clone https://github.com/asterisk/asterisk.git
cd asterisk
sudo git checkout 20.0.1
sudo ./configure
sudo make
sudo make install
sudo make samples
sudo make config
sudo make install-logrotate
```

4. **Vérifier le statut du service Asterisk :**

   ```bash
   sudo systemctl status asterisk
   ```

### 2. Configurer l'utilisateur

Pour créer et configurer un utilisateur SIP dans Asterisk, vous devrez éditer les fichiers de configuration d'Asterisk. 

1. **Créer et configurer l'utilisateur dans le fichier `sip.conf` :**

   Ouvrez le fichier `sip.conf` pour l'édition :

   ```bash
   sudo nano /etc/asterisk/sip.conf
   ```

   Ajoutez la configuration suivante pour l'utilisateur `sgroot` :

   ```ini
   [sgroot]
   type=friend
   secret=0000
   context=Marketing
   host=dynamic
   voicemail=88012@ff
   callerid="Stéphane Groot" <88012>
   mailbox=88012
   ; Configurez les codecs
   disallow=all
   allow=ulaw
   ```

   **Explication :**
   - `type=friend` : Configure l'utilisateur pour la fois en tant que client SIP et pour accepter des appels.
   - `secret=0000` : Définit le mot de passe de l'utilisateur.
   - `context=Marketing` : Place l'utilisateur dans le contexte "Marketing".
   - `host=dynamic` : L'adresse IP est dynamique.
   - `voicemail=88012@ff` : Configure l'adresse de messagerie vocale.
   - `callerid="Stéphane Groot" <88012>` : Définit l'identité de l'appelant.
   - `mailbox=88012` : Spécifie la boîte vocale.

2. **Configurer la messagerie vocale dans le fichier `voicemail.conf` :**

   Ouvrez le fichier `voicemail.conf` :

   ```bash
   sudo nano /etc/asterisk/voicemail.conf
   ```

   Ajoutez la configuration suivante :

   ```ini
   [default]
   88012 => 1234,Stéphane Groot,88012@ff
   ```

   **Explication :**
   - `88012` : Le numéro de la boîte vocale.
   - `1234` : Code secret de la messagerie vocale.
   - `Stéphane Groot` : Nom complet.
   - `88012@ff` : Adresse de la messagerie vocale.

### 3. Redémarrer Asterisk

Pour appliquer les changements, redémarrez Asterisk :

```bash
sudo systemctl restart asterisk
```

### 4. Vérifier la configuration

Vous pouvez utiliser la CLI d'Asterisk pour vérifier si l'utilisateur est bien configuré et si Asterisk fonctionne correctement :

```bash
sudo asterisk -rvv
```

Cela vous permet de voir les logs en temps réel et d'effectuer des vérifications supplémentaires si nécessaire.

