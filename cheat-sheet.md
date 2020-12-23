# Cheat sheet

Hou in dit bestand de belangrijkste commando's bij die je tegenkomt, zodat je die snel kan terugvinden. Steek structuur in het bestand en let op een [correct gebruik van Markdown](https://help.github.com/articles/getting-started-with-writing-and-formatting-on-github/) zodat het overzichtelijk blijft. Voor inspiratie en motivatie voor het bijhouden van dit soort cheat sheets, ga eens kijken naar <https://github.com/bertvv/cheat-sheets/>.

## Markdown

|         Doel         | Syntax              |
| :------------------: | :------------------ |
|       Hoofding       | \# H1               |
|                      | \## H2              |
|                      | \### H3             |
|                      | \#### H4 (tot 6)    |
|     Vette tekst      | \*\*vette tekst\*\* |
|    Schuine tekst     | \*schuine tekst\*   |
|      Blockquote      | \> blockquote       |
|   geordende lijst    | 1. eerste item      |
|                      | 2. tweede item      |
|                      | 3. derde item       |
| niet-geordende lijst | \- eerste item      |
|                      | \- tweede item      |
|                      | \- derde item       |
|         code         | \`code\`            |
| markdown tag negeren | `\# H1`             |

## Git Bash

### Zijn er changes?

```console
git status
```

### Changes pushen

1. Changes stagen

```console
git add . ( '.' voor hele directory)
```

2. Commit maken

```console
git commit -m "mijn beschrijving"
```

3. Commit pushen

```console
git push
```
