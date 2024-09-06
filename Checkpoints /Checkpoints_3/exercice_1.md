## Exercice1

## Partie 1 : Gestion des utilisateurs

### Q.1.1.1 Créer l'utilisateur Lionel Lemarchand avec les mêmes attributs de société que Kelly Rhameur.

1. **Ouvrir la console Active Directory Users and Computers (ADUC)** :
   - Cliquez sur Démarrer > Outils d'administration > Utilisateurs et ordinateurs Active Directory.
   
2. **Localiser l'utilisateur Kelly Rhameur** :
   - Naviguez dans l'OU où Kelly Rhameur se trouve.

3. **Copier l'utilisateur** :
   - Cliquez avec le bouton droit sur Kelly Rhameur, puis sélectionnez "Copier".
   - Remplissez les informations pour Lionel Lemarchand (prénom, nom, nom d'utilisateur).
   - Cliquez sur "Suivant" et remplissez les champs nécessaires. Cliquez sur "Terminer".

![Capture d'écran 2024-09-06 092158](https://github.com/user-attachments/assets/4103a435-23b5-4f76-8209-e856147ad6d5)

![Capture d'écran 2024-09-06 092207](https://github.com/user-attachments/assets/57815713-630d-4640-884a-63f2bb6119a1)

### Q.1.1.2 Créer une OU `DesactivatedUsers` et déplacer le compte désactivé de Kelly Rhameur dedans.

1. **Créer une nouvelle OU** :
   - Faites un clic droit sur le domaine ou l'OU parent souhaitée.
   - Sélectionnez "Nouveau" > "Unité d'organisation".
   - Nommez-la `DesactivatedUsers`.

2. **Désactiver le compte de Kelly Rhameur** :
   - Cliquez avec le bouton droit sur le compte de Kelly Rhameur, puis sélectionnez "Désactiver le compte".

3. **Déplacer le compte de Kelly Rhameur** :
   - Faites glisser le compte de Kelly Rhameur dans la nouvelle OU `DesactivatedUsers`, ou faites un clic droit sur le compte, sélectionnez "Déplacer", puis choisissez `DesactivatedUsers` comme destination.

![Capture d'écran 2024-09-06 104252](https://github.com/user-attachments/assets/0bc6f572-61d7-4da0-9146-50aa1016986e)

### Q.1.1.3 Modifier le groupe de l'OU dans laquelle était Kelly Rhameur en conséquence.

1. **Accéder au groupe d'OU** :
   - Localisez le groupe qui est spécifique à l'OU où Kelly Rhameur était située.

2. **Retirer Kelly Rhameur du groupe** :
   - Ouvrez le groupe et retirez Kelly Rhameur de la liste des membres.

![Capture d'écran 2024-09-06 092229](https://github.com/user-attachments/assets/09876edc-4361-4baf-be8f-7ddcd34d63ab)

3. **Ajouter Lionel Lemarchand au groupe** :
   - Ajoutez Lionel Lemarchand en tant que membre du groupe pour lui attribuer les mêmes autorisations que celles dont Kelly Rhameur bénéficiait.

![Capture d'écran 2024-09-06 104438](https://github.com/user-attachments/assets/3dc55861-bb35-4185-9f89-cf80a925139c)

### Q.1.1.4 Créer le dossier Individuel du nouvel utilisateur et archiver celui de Kelly Rhameur en le suffixant par `-ARCHIVE`.

1. **Créer le dossier de Lionel Lemarchand** :
   - Naviguez jusqu'à l'emplacement où les dossiers individuels sont stockés.
   - Créez un nouveau dossier nommé `Lionel Lemarchand`.

2. **Archiver le dossier de Kelly Rhameur** :
   - Localisez le dossier de Kelly Rhameur.
   - Renommez le dossier `Kelly Rhameur-ARCHIVE`.

## Partie 2 : Restriction utilisateurs

### Q.1.2.1 : Restreindre les heures de connexion pour l'utilisateur Gabriel Ghul


1. **Configurer les heures de connexion** :
   - Faites un clic droit sur le compte utilisateur **Gabriel Ghul** et sélectionnez **Propriétés**.
   - Allez à l'onglet **Compte**.
   - Cliquez sur **Heures de connexion**.
   - Configurez les heures de connexion autorisées :
     - **Lundi à Vendredi** : Cochez les jours de la semaine.
     - **Heures** : Configurez de 07:00 à 17:00 pour chaque jour sélectionné.

![Capture d'écran 2024-09-06 093336](https://github.com/user-attachments/assets/7e44d19f-177a-4d20-a1bb-0ba0b409b2e2)

### Q.1.2.2 : Bloquer la connexion de Gabriel Ghul à l'ordinateur CLIENT01

![Capture d'écran 2024-09-06 093415](https://github.com/user-attachments/assets/e1a3b93b-08ab-48d9-a53b-0104b23e6004)


###  Q.1.2.3  Mettre en Place une Stratégie de Mot de Passe

1. **Ouvrir la Console de Gestion des Stratégies de Groupe (GPMC)** :
   - Cliquez sur Démarrer et tapez `Gestion des stratégies de groupe`.

2. **Créer une Nouvelle Stratégie de Groupe (GPO)** :
   - Faites un clic droit sur l'OU `LabUsers` dans l'arborescence de votre domaine.
   - Sélectionnez "Créer un objet GPO dans ce domaine, et le lier ici".
   - Donnez un nom à la nouvelle GPO, par exemple, "Stratégie de Mot de Passe LabUsers".

![Capture d'écran 2024-09-06 093541](https://github.com/user-attachments/assets/439e7c86-7ba2-47fa-8e01-fe0b4662510d)

![Capture d'écran 2024-09-06 093609](https://github.com/user-attachments/assets/e8bc417a-48ce-4154-af8c-b6eea1edeca3)

3. **Configurer les Paramètres de Mot de Passe** :
   - Faites un clic droit sur la GPO que vous venez de créer et sélectionnez "Modifier".
   - Dans l'Éditeur de gestion des stratégies de groupe, accédez à :  
     **Configuration de l'ordinateur** > **Stratégies** > **Paramètres Windows** > **Paramètres de sécurité** > **Stratégies de compte** > **Stratégie de mot de passe**.

4. **Configurer les Options de la Stratégie de Mot de Passe** :
   - **Longueur minimale du mot de passe** : Augmentez cette valeur pour exiger des mots de passe plus longs, par exemple 12 caractères.
   - **Complexité du mot de passe** : Activez cette option pour exiger que les mots de passe contiennent des lettres majuscules, minuscules, des chiffres et des symboles.
   - **Historique du mot de passe** : Configurez cette option pour empêcher l'utilisateur de réutiliser les anciens mots de passe (par exemple, 24 anciens mots de passe).
   - **Durée minimale du mot de passe** : Vous pouvez définir une durée minimale pour que les utilisateurs ne changent pas immédiatement leur mot de passe (par exemple, 1 jour).
   - **Durée de vie maximale du mot de passe** : Réduisez cette durée pour forcer les utilisateurs à changer leur mot de passe plus souvent (par exemple, tous les 60 jours).
   - **Mot de passe réversible** : Désactivez cette option pour éviter de stocker des mots de passe sous une forme réversible.

**Exemple :**

![Capture d'écran 2024-09-06 093738](https://github.com/user-attachments/assets/7d4b3731-b2e6-4436-a9f3-1c558ae7e088)

![Capture d'écran 2024-09-06 093858](https://github.com/user-attachments/assets/90829734-5292-4e30-b89a-dd97b3a399bd)

## Partie 3 : Lecteurs réseaux

###  Q.1.3.1  Utiliser un Script de Connexion

Vous pouvez utiliser un script de connexion pour mapper les lecteurs réseau :

1. **Créer un Script Batch (.bat)** :
   - Ouvrez un éditeur de texte (comme Notepad) et écrivez le script suivant :

     ```
     net use E: \\serveur\partageE /persistent:YES
     net use F: \\serveur\partageF /persistent:YES
     ```

![Capture d'écran 2024-09-06 095140](https://github.com/user-attachments/assets/d493f818-e6db-44a7-8e3b-c323e808db06)

- Enregistrer le fichier avec l'extension `.bat`.

2. **Appliquer le Script via une GPO** :
   - Dans l'**Éditeur de Gestion des Stratégies de Groupe** :
     - Naviguez vers **User Configuration** > **Policies** > **Windows Settings** > **Scripts (Logon/Logoff)** > **Logon**.
     - Ajoutez le script batch que vous avez créé.

![Capture d'écran 2024-09-06 095148](https://github.com/user-attachments/assets/d3fb8a5f-c228-412e-b49e-a82f7b4087be)

![Capture d'écran 2024-09-06 095213](https://github.com/user-attachments/assets/75ebae34-ea52-4c1f-8b9b-9df7d9fac2cd)


