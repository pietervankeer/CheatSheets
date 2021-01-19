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

### 5.2 Reservaties

#### 5.2.1 Reservatie toevoegen

**_Manueel_**

via Server manager --> Tools --> DHCP --> Scope --> Reservations

**_Powershell_**

```powershell
Add-DhcpServerv4Reservation -ClientId "F0-DE-F1-7A-00-5E" -IPAddress 10.10.10.8 -ScopeId "192.168.1.0" -Description "Reservatie voor printer" -Name "ReservatieNaam"
```

> _ClientId: mac-adress van de Client_

> _Reservaties toevoegen vanuit een bestand:_
>
> ```powershell
> Import-Csv -Path "Reservations.csv" | Add-DhcpServerv4Reservation -ComputerName "dhcpserver.contoso.com"
> ```

#### 5.2.2 Reservatie Bewerken

**_Manueel_**

via Server manager --> Tools --> DHCP --> Scope --> Reservations

**_Powershell_**

```powershell
Set-DhcpServerv4Reservation -IPAddress 10.10.10.8 -Description "Reservatie voor een scanner" -Name nieuweReservatieNaam
```

#### 5.2.3 Reservatie Verwijderen

**_Manueel_**

via Server manager --> Tools --> DHCP --> Scope --> Reservations

**_Powershell_**

```powershell
Remove-DhcpServerv4Reservation -IPAddress 10.10.10.8
```

## H7: Gedeelde mappen

### 7.1 Shares bekijken

1. via MMC **Computer management**
2. Via server manager: _Files and Storage Services_ --> _Shares_
3. UNC notatie: In File explorer --> zoeken naam `\\_servernaam_\_share_`

### 7.2 Shares aanmaken

**_Manueel_**

Via Computer management met _new share wizard_

**_Powershell_**

```powershell
New-SmbShare -Name ShareNaam -Path F:\UserFolders
```

### 7.3 Shares verwijderen

**_Manueel_**

Via Computer management

**_Powershell_**

```powershell
Remove-SmbShare -Name ShareNaam
```

## H8: Structureren van gebruikers

### 8.1 Aanmaken van OU's (Organisational Units)

**_Manueel_**

Via AD users and computers

**_Powershell_**

Onderstaand commando maakt binnen de OU **Afdelingen** een nieuwe OU **Administratie**

```powershell
New-ADOrganizationalUnit -Name "Administratie" -Path "ou=Afdelingen,DC=confidas,DC=local" -ProtectedFromAccidentalDeletion $False
```

### 8.2 Verwijderen OU

**_Manueel_**

Via AD users and computers

**_Powershell_**

```powershell
Remove-ADOrganizationalUnit "ou=Administratie,ou=Afdelingen,DC=confidas,DC=local"
```

### 8.3 Aanmaken van Gebruikers

**_Manueel_**

Via AD users and computers

**_Powershell_**

Onderstaand commando maakt binnen de OU **Afdelingen** een nieuwe OU **Administratie**

```powershell
New-ADUser -Path "ou=Directie,ou=Afdelingen,DC=confidas,DC=local"  -Name "Jolando Brands" -Description "Adjunct Automatisering" -Title "Adjunct Directeur" -AccountPassword "jol&bra" -ProfilePath "\\winserver1\UserProfiles\%username%" -HomeDirectory "\\winserver1\UserFolders\%username%" -UserPrincipalName "jol_bra"
```

### 8.4 Verwijderen Gebruikers

**_Manueel_**

Via AD users and computers

**_Powershell_**

```powershell
Remove-ADUser -Identity "CN=Jolanda Brands,OU=Directie,ou=afdelingen,DC=confidas,DC=local"
```

### 8.5 bewerken Gebruikers

**_Manueel_**

Via AD users and computers

**_Powershell_**

```powershell
Set-ADUser -Identity (CN=Jolanda Brands,OU=Directie,ou=afdelingen,DC=confidas,DC=local) -DisplayName "jolanda branden" -Manager (Madelief Smets)
```
