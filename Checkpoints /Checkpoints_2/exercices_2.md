### Q.2.2 : Problème code fichier Main.ps1

Il se passe rien , le script ne se lance pas.

**Modification :**

```powershell
$scriptPath = "C:\Users\wilder\Downloads\AddLocalUsers.ps1"

# Vérifier si le script existe avant de tenter de l'exécuter
if (Test-Path $scriptPath) {
    Start-Process -FilePath "powershell.exe" -ArgumentList "-File `"$scriptPath`"" -Verb RunAs -WindowStyle Maximized
} else {
    Write-Host "Le fichier de script n'existe pas à l'emplacement spécifié." -ForegroundColor Red
}
```
Utilisation Incorrecte de `-ArgumentList` donc je l'ai remplacé par `-File`, il y avais un mauvais chemin donc je l'ai remplacé par `"C:\Users\wilder\Downloads\AddLocalUsers.ps1"` et il n'y a pas de vérification si il y a déjà le script ou non donc j'ai rajouté un `if`.

### Q.2.3 : À quoi sert l'option `-Verb RunAs` ?

L'option `-Verb RunAs` dans la commande `Start-Process` est utilisée pour exécuter un processus avec des privilèges élevés, c'est-à-dire en tant qu'administrateur. 


### Q.2.4 : À quoi sert l'option `-WindowStyle Maximized` ?

L'option `-WindowStyle Maximized` dans la commande `Start-Process` spécifie l'état de la fenêtre du processus lancé. 


### Q.2.5 : Le premier utilisateur du fichier Users.csv n'est jamais pris en compte

L'utilisation de `Select-Object -Skip 2` saute les deux premières lignes du fichier CSV. 

**Modification :**

```powershell
$Users = Import-Csv -Path $CsvFile -Delimiter ";" -Header "prenom","nom","societe","fonction","service","description","mail","mobile","scriptPath","telephoneNumber" -Encoding UTF8


### Q.2.6 : Le champ `Description` est importé du fichier CSV mais n'est pas utilisé

Le champ `Description` est importé mais n'est pas inclus dans les informations de création d'utilisateur.

**Modification :**

```powershell
$UserInfo = @{
    Name                 = "$Prenom.$Nom"
    FullName             = "$Prenom.$Nom"
    Description          = $Description
    Password             = $Password
    AccountNeverExpires  = $true
    PasswordNeverExpires = $false
}
```

### Q.2.7 : Importation des utilisateurs avec tous les champs alors qu'une partie est utilisée

Tous les champs du fichier CSV sont importés, mais il y a qu'une partie des fichiers utilisés.

**Modification :**

```powershell
$Users = Import-Csv -Path $CsvFile -Delimiter ";" -Header "prenom","nom","description" -Encoding UTF8
```

### Q.2.8 : Afficher le mot de passe créé

**Modification :**

```powershell
Write-Host "Le compte $Prenom.$Nom a été créé avec le mot de passe $Pass" -ForegroundColor Green
```

### Q.2.9 : Utiliser la fonction `Log` du fichier `Functions.psm1`

**Méthodes pour utiliser la fonction `Log` :**

1. **Importation du module et appel de la fonction :**

   ```powershell
   Import-Module "$Path\Functions.psm1"
   Log -Message "Début du traitement des utilisateurs" -LogFile $LogFile
   ```

2. **Directement dans le script après importation :**

   ```powershell
   . "$Path\Functions.psm1"
   Log -Message "Début du traitement des utilisateurs" -LogFile $LogFile
   ```

**Modification du script avec une méthode :**

```powershell
. "$Path\Functions.psm1"
Log -Message "Début du traitement des utilisateurs" -LogFile $LogFile
```

### Q.2.10 : Afficher un message si l'utilisateur existe déjà

**Modification :**

```powershell
if (Get-LocalUser -Name "$Prenom.$Nom" -ErrorAction SilentlyContinue) {
    Write-Host "Le compte $Prenom.$Nom existe déjà" -ForegroundColor Red
} else {
    New-LocalUser @UserInfo
    Add-LocalGroupMember -Group "Utilisateur" -Member "$Prenom.$Nom"
    Write-Host "L'utilisateur $Prenom.$Nom a été créé avec le mot de passe $Pass" -ForegroundColor Green
}
```

### Q.2.11 : Ajout des utilisateurs dans le groupe des utilisateurs locaux

**Modification :**

```powershell
Add-LocalGroupMember -Group "Users" -Member "$Prenom.$Nom"
```

### Q.2.12 : Remplacer `$Prenom.$Nom` par `$Name`

**Modification :**

```powershell
$Name = "$Prenom.$Nom"
If (-not(Get-LocalUser -Name $Name -ErrorAction SilentlyContinue))
{
    $Pass = Random-Password
    $Password = (ConvertTo-SecureString $Pass -AsPlainText -Force)
    $Description = "$($User.description) - $($User.fonction)"
    $UserInfo = @{
        Name                 = $Name
        FullName             = $Name
        Description          = $Description
        Password             = $Password
        AccountNeverExpires  = $true
        PasswordNeverExpires = $false
    }

    New-LocalUser @UserInfo
    Add-LocalGroupMember -Group "Users" -Member $Name
    Write-Host "L'utilisateur $Name a été créé avec le mot de passe $Pass" -ForegroundColor Green
}
```

### Q.2.13 : Le mot de passe n'expire pas

**Modification :**
Dans la définition du mot de passe, `AccountNeverExpires` est bien à `$true` et maintenant `PasswordNeverExpires`  est à `$true` :

```powershell
$UserInfo = @{
    Name                 = $Name
    FullName             = $Name
    Description          = $Description
    Password             = $Password
    AccountNeverExpires  = $true
    PasswordNeverExpires = $true
}
```

### Q.2.14 : Mot de passe de 12 caractères

**Modification :**

```powershell
Function Random-Password ($length = 12)
```

### Q.2.15 : Remplacer la pause par une attente de l'entrée de l'utilisateur

**Modification :**

```powershell
Read-Host -Prompt "Appuyez sur Entrée pour continuer..."
```

### Q.2.16 : À quoi sert la fonction `ManageAccentsAndCapitalLetters` ?

**Explication :**
La fonction `ManageAccentsAndCapitalLetters` supprime les accents des caractères et convertit la chaîne en minuscules.

**Exemple :**

Pour l'utilisateur : "Mathéo Aubert" :

```powershell
$Prenom = ManageAccentsAndCapitalLetters -String "Mathéo"
$Nom = ManageAccentsAndCapitalLetters -String "Aubert"
# Résultat : Mathéo -> matheo, Aubert -> aubert
```


