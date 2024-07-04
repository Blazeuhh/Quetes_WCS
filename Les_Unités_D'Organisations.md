# Création d'une Unité d'Organisation, d'un Groupe d'Utilisateurs, et d'un Utilisateur sous Windows Server 2012 R2

## Étape 1 : Ouvrir les Outils de Gestion AD

1. **Ouvrir le Gestionnaire de serveur :**
   - Cliquez sur le bouton "Démarrer", puis sélectionnez "Gestionnaire de serveur".
   - Dans le Gestionnaire de serveur, cliquez sur "Outils" > "Utilisateurs et ordinateurs Active Directory".

## Étape 2 : Créer une Unité d'Organisation (OU)

1. **Ouvrir la console AD :**
   - Dans "Utilisateurs et ordinateurs Active Directory", naviguez jusqu'au domaine (par exemple, `wilders.lan`).

2. **Créer une nouvelle OU :**
   - Clic droit sur le domaine (par exemple, `wilders.lan`) > "Nouveau" > "Unité d'Organisation".
   - Nom : `Wilders_students`.
   - Cliquez sur "OK".

## Étape 3 : Créer un Groupe d'Utilisateurs

1. **Naviguer vers l'OU Wilders_students :**
   - Développez le domaine jusqu'à l'OU `Wilders_students`.

2. **Créer un nouveau groupe :**
   - Clic droit sur l'OU `Wilders_students` > "Nouveau" > "Groupe".
   - Nom : `Students`.
   - Type de groupe : "Sécurité".
   - Portée : "Global".
   - Cliquez sur "OK".

## Étape 4 : Créer un Utilisateur au sein du Groupe

1. **Naviguer vers l'OU Wilders_students :**
   - Développez le domaine jusqu'à l'OU `Wilders_students`.

2. **Créer un nouvel utilisateur :**
   - Clic droit sur l'OU `Wilders_students` > "Nouveau" > "Utilisateur".
   - Remplir les champs "Prénom", "Initiale" et "Nom".
     - Exemple : Prénom : `John`, Nom : `Doe`.
   - Nom d'ouverture de session : `jdoe`.
   - Cliquez sur "Suivant".

3. **Définir le mot de passe de l'utilisateur :**
   - Entrez un mot de passe et confirmez-le.
   - Options de mot de passe : "L'utilisateur doit changer le mot de passe à la prochaine ouverture de session".
   - Cliquez sur "Suivant" > "Terminer".

4. **Ajouter l'utilisateur au groupe :**
   - Trouvez l'utilisateur créé (par exemple, `John Doe`).
   - Clic droit sur l'utilisateur > "Propriétés".
   - Onglet "Membre de" > "Ajouter...".
   - Entrez le nom du groupe (par exemple, `Students`) > "OK".
   - Cliquez sur "Appliquer" > "OK".
