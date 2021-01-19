# Windows Server 2019

## Algemeen

### Shortcuts

| toetsen-combinatie | doel                               |
| :----------------- | :--------------------------------- |
| Windows + e        | opent explorer (windows verkenner) |
| Windows + d        | verberg alle openstaande vensters  |
| Windows + r        | opent run                          |

### Commando's in run

| commando     | doel                                 |
| :----------- | :----------------------------------- |
| ncpa.cpl     | opent de netwerkadapters             |
| services.mcs | opent de services                    |
| cmd.exe      | opent de command prompt              |
| dca.msc      | Active directory users and computers |
| compmgmt.msc | Computer management                  |
| diskmgmt.msc | Schijfbeheer                         |

### Back-up nemen van de (virtuele) harde schijf

Ga naar de locatie waar deze is opgeslagen en kopieer naar een gezipte map.

## H4: Configuratie DNS en routing na installatie Active Directory

### 4.1 Forward Lookup Zone

Een forward lookup zone wordt hoofdzakelijk gebruikt om DNS-namen (vb: **winserver1.confidas.local**) om te zetten naar een IP adres (vb: **192.168.1.1**). Hiervoor worden **A-records** gebruikt, maar je kan ook aliassen (**CNAME**-records) toevoegen

### 4.2 Reverse Lookup Zone

Een _Reverse Lookup Zone_ doet het omgekeerde als een _Forward Lookup Zone_, en wordt dus hoofdzakelijk gebruikt om aan de hand van een IP adres een hostname op te zoeken. Zo kan je via een reverse lookup zone achterhalen dat het IP-adres 192.168.1.1 behoort tot Winserver1.confidas.local. Hiervoor worden **PTR-records** gebruikt. De reverse lookup zone moeten we zelf nog aanmaken en configureren.

#### 4.2.1 **PTR-records** aanmaken

Laten genereren via de forward lookup zone **A-records** --> properties

## H5: DHCP configuratie

### 5.1 Scope aanmaken

scope aanmaken en daarna (om in werking te treden) autoriseren en activeren

## H7: Gedeelde mappen

### 7.1 Shares bekijken

1. via MMC **Computer management**
2. Via server manager: _Files and Storage Services_ --> _Shares_
3. UNC notatie: In File explorer --> zoeken naam `\\_servernaam_\_share_`

### 7.2 Shares aanmaken

#### Manueel

Via Computer management met _new share wizard_

#### PowerShell

```powershell
INSERT CODE HIER JA
```

### 7.3 Shares verwijderen

#### Manueel

Via Computer management

#### PowerShell

```powershell
INSERT CODE HIER JA
```

## H8: Structureren van gebruikers

### 8.1 Aanmaken van OU's (Organisational Units)

#### Manueel

Via AD users and computers

#### PowerShell

Onderstaand commando maakt binnen de OU **Afdelingen** een nieuwe OU **Administratie**

```powershell
New-ADOrganizationalUnit -Name "Administratie" -Path "ou=Afdelingen,DC=confidas,DC=local" -ProtectedFromAccidentalDeletion $False
```

### 8.2 Verwijderen OU

#### Manueel

Via AD users and computers

#### PowerShell

```powershell
Remove-ADOrganizationalUnit "ou=Administratie,ou=Afdelingen,DC=confidas,DC=local"
```

### 8.1 Aanmaken van OU's (Organisational Units)

#### Manueel

Via AD users and computers

#### PowerShell

Onderstaand commando maakt binnen de OU **Afdelingen** een nieuwe OU **Administratie**

```powershell
New-ADOrganizationalUnit -Name "Administratie" -Path "ou=Afdelingen,DC=confidas,DC=local" -ProtectedFromAccidentalDeletion $False
```

### 8.3 Aanmaken van Gebruikers

#### Manueel

Via AD users and computers

#### PowerShell

Onderstaand commando maakt binnen de OU **Afdelingen** een nieuwe OU **Administratie**

```powershell
New-ADUser -Path "ou=Directie,ou=Afdelingen,DC=confidas,DC=local"  -Name "Jolando Brands" -Description "Adjunct Automatisering" -Title "Adjunct Directeur" -AccountPassword "jol&bra" -ProfilePath "\\winserver1\UserProfiles\%username%" -HomeDirectory "\\winserver1\UserFolders\%username%" -UserPrincipalName "jol_bra"
```

### 8.4 Verwijderen Gebruikers

#### Manueel

Via AD users and computers

#### PowerShell

```powershell
Remove-ADUser -Identity "CN=Jolanda Brands,OU=Directie,ou=afdelingen,DC=confidas,DC=local"
```

### 8.5 bewerken Gebruikers

#### Manueel

Via AD users and computers

#### PowerShell

```powershell
Remove-ADUser -Identity "CN=Jolanda Brands,OU=Directie,ou=afdelingen,DC=confidas,DC=local"
```
