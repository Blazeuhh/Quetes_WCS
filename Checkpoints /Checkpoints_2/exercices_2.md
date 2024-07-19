
### Q.2.5 : Le premier utilisateur du fichier Users.csv n'est jamais pris en compte

L'utilisation de `Select-Object -Skip 2` saute les deux premières lignes du fichier CSV. 

**Modification :**

```powershell
$Users = Import-Csv -Path $CsvFile -Delimiter ";" -Header "prenom","nom","societe","fonction","service","description","mail","mobile","scriptPath","telephoneNumber" -Encoding UTF8
```

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
Assurez-vous que le groupe est correctement nommé et que le compte utilisateur est ajouté :

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
Dans la définition du mot de passe, assurez-vous que `AccountNeverExpires` est bien à `$true` et `PasswordNeverExpires` à `$true` :

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

Pour la liste des utilisateurs avec des prénoms et noms accentués comme "René Dupont" :

```powershell
$Prenom = ManageAccentsAndCapitalLetters -String "René"
$Nom = ManageAccentsAndCapitalLetters -String "Dupont"
# Résultat : René -> Rene, Dupont -> dupont
```


Cela devrait résoudre les problèmes que vous avez identifiés et répondre à toutes les questions posées.
