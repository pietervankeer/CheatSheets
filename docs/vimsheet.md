# Vim Cheatsheet

## Bestanden aanmaken

```bash
\$ Vim naam_bestand
```

## Van _normal mode_ naar _insert mode_?

| Invoegen vanaf                  | Commando |
| :------------------------------ | :------- |
| op de huidige cursorpositie     | `i`      |
| rechts van de cursor            | `a`      |
| begin van huidige regel         | `I`      |
| einde van huidige regel         | `A`      |
| regel toevoegen onder deze      | `o`      |
| regel toevoegen op huidige lijn | `O`      |

## Kopieren vanuit _normal mode_

| Te kopiëren                              | Commando |
| :--------------------------------------- | :------- |
| Huidige regel                            | `yy`     |
| Huidige regel en die eronder             | `2yy`    |
| Het huidige woord                        | `yw`     |
| Het huidige en de twee volgende woorden  | `2yw`    |
| Van de cursor tot het einde van de regel | `y$`     |
| Tot het einde van de _zin_               | `yas`    |
| Tot het einde van de _paragraaf_         | `yap`    |
| Alle tekst tussen haakjes `(...)`        | `yi(`    |

## Knippen vanuit _normal mode_

| Te knippen                                  | Commando |
| :------------------------------------------ | :------- |
| Huidige regel                               | `dd`     |
| Huidige regel en die eronder                | `2dd`    |
| Het huidige woord                           | `dw`     |
| Het huidige en de twee volgende woorden     | `2dw`    |
| Het letterteken op de positie van de cursor | `x`      |
| Van de cursor tot het einde van de regel    | `d$`     |
| Tot het einde van de _zin_                  | `das`    |
| Tot het einde van de _paragraaf_            | `dap`    |
| Alle tekst tussen haakjes `(...)`           | `di(`    |

## Hoe kan je gekopieerde/geknipte tekst plakken?

| Tekst plakken          | Commando |
| :--------------------- | :------- |
| Rechts/onder de cursur | `p`      |
| Op de cursur           | `P`      |
