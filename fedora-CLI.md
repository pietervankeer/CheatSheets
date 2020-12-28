# Fedora - Command Line Interface

## Hulp zoeken

man = manual

| Commando      | Verklaring                                     |
| :------------ | :--------------------------------------------- |
| man passwd    | hulp over het commando passwd                  |
| man 5 passwd  | Hulp over het configuratiebestand /etc/passwd  |
| man -k passwd | zoek in alle man-pages naar de string 'passwd' |

### binnen de man-page

| Commando | Verklaring                     |
| :------- | :----------------------------- |
| q        | man-page verlaten              |
| /        | zoeken binnen de pagina        |
| n        | ga naar volgende zoekresultaat |
| N        | ga naar vorige zoekresultaat   |

### secties

| Commando | Verklaring              |
| :------- | :---------------------- |
| 1        | commando's              |
| 5        | configuratiebestanden   |
| 8        | systeembeheercommando's |

vb:  
`passwd(1), passwd(5)`

### belangrijke man-pages

die niet over een commando gaan

| Commando     | Verklaring                                      |
| :----------- | :---------------------------------------------- |
| man hier     | directorystructuur Linux (filesystem hierarchy) |
| man builtins | "ingebouwde" bash commando's                    |
| man 7 glob   | "wildcards" in bestandsnamen (bv: \*.\*, [a-z]) |

## Werken met directory's

### speciale directorynamen

| Commando | Verklaring                             |
| :------- | :------------------------------------- |
| /        | root                                   |
| .        | de huidige directory (pwd)             |
| ..       | De bovenliggende directory             |
| ~        | je "home-directory (vb: /home/student) |

> _opgelet!_ De "home directory" van de gebruiker "root" is `/root`

| Commando | Verklaring                       |
| :------- | :------------------------------- |
| pwd      | Toon huidige directory           |
| ls       | Toon de inhoud huidige directory |
| cd       | ga naar een andere directory     |
| mkdir    | Maak een subdirectory aan        |
| rmdir    | verwijder een lege directory     |

## Werken met bestanden

| Commando | Verklaring                                                         |
| :------- | :----------------------------------------------------------------- |
| cat      | Toon de inhoud van een bestand                                     |
| less     | Toon inhoud, per pagina (navigeer met pijltjes (less is more))     |
| touch    | Maak leeg bestand aan (eigenlijk: pas datum laatste wijziging aan) |
| cp       | kopieer bestanden                                                  |
| mv       | verplaats bestanden ( of hernoemen)                                |
| rm       | verwijder bestanden of directories                                 |

## Globbing

Meerdere bestanden opgeven -> Globbing.

vb:  
`cp a.txt b.doc c.jpg /tmp`  
of  
`cp /media/usbstick/*.jpg ~/Pictures/`

### Globbing-patronen

| Commando | Verklaring                          | voorbeeld          |
| :------- | :---------------------------------- | :----------------- |
| ?        | eén willekeurig teken               | ls /bin/??         |
| \*       | willekeurige string (ook leeg)      | ls \*.txt , ls a\* |
| [...]    | elke teken opgesomd tussen [ ]      | ls /bin/[A+_]\*    |
| [A-Z]    | Van A tot en met Z                  | ls /bin/\*[A-D1-3] |
| [!...]   | **_niet_** 1 van de opgesomde reeks | ls /bin/[!a-z]     |

> _opgelet!_ Globbing is niet hetzelfde als reguliere expressies

- Globbing:
  - bestandsnamen opgeven
  - **case** statement in Bash scripting

## Werken met tekst

### Te kennen commando's

| Commando | Doel                                                             |
| :------- | :--------------------------------------------------------------- |
| awk      | Veelzijdige tool voor bewerken van tekst                         |
| cat      | druk inhoud bestand(en) af op _stdout_                           |
| cut      | selecteer "kolommen" uit tekstbestanden                          |
| fmt      | herformatteer tekst (vb: aantal kolommen)                        |
| grep     | zoek adhv reguliere expressies naar tekstpatronen in bestanden   |
| head     | toon de eerste regels van een tekstbestand                       |
| join     | voeg twee tekstbestanden samen adhv een gemeenschappelijke kolom |
| nl       | voeg regelnummers toe                                            |
| paste    | voeg twee tekstbestanden regel per regel samen                   |
| sef      | veelzijdige tool voor bewerken van tekst (Stream Editor)         |
| trail    | toon de laatste regels van een tekstbestand                      |
| tr       | zoek en vervang lettertekens in tekst                            |
| uniq     | verwijder dubbele rijen uit een gesorteerd tekstbestand          |
| wc       | tel karakters, woorden of lijnen in een tekstbestand             |

### I/O redirection

| syntax       | betekenis                                                     |
| :----------- | :------------------------------------------------------------ |
| cmd > file   | Schrijf uitvoer van _cmd_ weg naar _file_                     |
| cmd >> file  | Voeg toe aan einde van _file_                                 |
| cmd 2> file  | schrijf foutboodschappen (_stderr_) van _cmd_ weg naar _file_ |
| cmd < file   | gebruik de inhoud van _file_ als invoer voor _cmd_            |
| cmd1 \| cmd2 | gebruikt de uitvoer van _cmd1_ als invoer voor _cmd2_         |

## combineren

| syntax                                          | betekenis                           |
| :---------------------------------------------- | :---------------------------------- |
| find / -type d > directorries.txt 2> errors.txt | stdout en stderr apart wegschrijven |
| find / -type d > directorries.txt 2> /dev/null  | stderr "negeren"                    |
| find / -type d > all.txt 2>&1                   | stdout en stderr samen wegschrijven |
| sort < unsorted.txt > sorted.txt 2> errors.txt  | invoer en uitvoer omleiden          |
