# Windows Server 2019

## Powershell

### Algemeen

| Task         | Command                                     |
| :----------- | :------------------------------------------ |
| Set Hostname | `Rename-Computer -NewName NEWNAME -restart` |


### Network

| Task                         | Command                                                                                      |
| :--------------------------- | :------------------------------------------------------------------------------------------- |
| Get specific Network Adapter | `Get-NetAdapter -Name "Ethernet 2"`                                                          |
| Get all Network Adapters     | `Get-NetAdapter -Name *`                                                                     |
| Rename Network Adapter       | `Rename-NetAdapter -Name "Ethernet" -NewName "LAN"`                                          |
| Disable ipv6                 | `Disable-NetAdapterBinding -Name "Adapter Name" -ComponentID ms_tcpip6`                      |
| Set ipv4 address             | `New-NetIPAddress -InterfaceAlias "Adapter name" -IPAddress 192.168.1.2 -AddressFamily IPv4` |


### Services

| Taak            | Commando                                                                |
| :-------------- | :---------------------------------------------------------------------- |
| View service    | `Get-Service &#124; Where-Object name -eq "service name" | Format-List` |
| Start service   | `Start-Service -Name name`                                              |
| Stop service    | `Stop-Service -Name name`                                               |
| Enable service  | `Set-service -Name "service name" -StartupType Automatic`               |
| Disable service | `Set-service -Name "service name" -StartupType Disable`                 |


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

#### 4.1.2 Records Toevoegen

**_Powershell_**

> _opgelet!_ Hieronder wordt een A-record aangemaakt

```powershell
Add-DnsServerResourceRecord -A -IPv4Address 172.18.99.33 -Name winserver1 -ZoneName confidas.local -CreatePtr
```

> _opgelet!_ Hieronder wordt een CNAME-record aangemaakt

```powershell
Add-DnsServerResourceRecord -CName -HostNameAlias winserver3 -Name winserver1 -ZoneName confidas.local
```

#### 4.1.3 Records Bewerken

**_Powershell_**

```powershell
$OldObj = Get-DnsServerResourceRecord -Name "192.168.20.21" -ZoneName "confidas.local" -RRType "A"
$NewObj = $OldObj.Clone()
$NewObj.TimeToLive = [System.TimeSpan]::FromHours(2)
Set-DnsServerResourceRecord -NewInputObject $NewObj -OldInputObject $OldObj -ZoneName "confidas.local"
HostName                  RecordType Timestamp            TimeToLive      RecordData
--------                  ---------- ---------            ----------      ----------
Host01                       A          0                    02:00:00        2.2.2.2
```

#### 4.1.4 Records Verwijderen

**_Powershell_**

```powershell
Remove-DnsServerResourceRecord -Name 192.168.20.21 -RRType A -ZoneName confidas.local
```

### 4.2 Reverse Lookup Zone

Een _Reverse Lookup Zone_ doet het omgekeerde als een _Forward Lookup Zone_, en wordt dus hoofdzakelijk gebruikt om aan de hand van een IP adres een hostname op te zoeken. Zo kan je via een reverse lookup zone achterhalen dat het IP-adres 192.168.1.1 behoort tot Winserver1.confidas.local. Hiervoor worden **PTR-records** gebruikt. De reverse lookup zone moeten we zelf nog aanmaken en configureren.

#### 4.2.1 **PTR-records** aanmaken

Laten genereren via de forward lookup zone **A-records** --> properties

**_Powershell_**

```powershell
Add-DnsServerResourceRecord -Name 192.168.20.21 -Ptr -PtrDomainName confidas.local -ZoneName confidas.local
```

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

> _ClientId: mac-adress van de Client_ > _Reservaties toevoegen vanuit een bestand:_
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