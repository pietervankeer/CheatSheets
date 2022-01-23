# Cheatsheet Python

## Output

| Syntax                        |                    Output                    |
| :---------------------------- | :------------------------------------------: |
| `print("Hello world!")`       |                 Hello world!                 |
| `print("{:.2f}".format(num))` | *float number formatted to 2 decimal places* |

## Converting Data Types

| Syntax   |                        |
| :------- | :--------------------: |
| `str(x)` | Converting to a string |
| `int(x)` |   converting to int    |

## Lists

| Syntax                              |                                   |
| :---------------------------------- | :-------------------------------: |
| `hostnames=["hello", "world", "!"]` |                                   |
| `len(hostnames)`                    |       length of the list: 3       |
| `hostnames[0]`                      |    value on index 0: **hello**    |
| `hostnames[-1]`                     |               **!**               |
| `del hostnames[1]`                  | delete item on index 1: **world** |

## Dictionaries

| Syntax                                            |       |
| :------------------------------------------------ | :---: |
| `ipAddress = { "R1":"10.1.1.1", "R2":"10.2.2.1"}` |       |

## Asking input

| Syntax                        |                          |
| :---------------------------- | :----------------------: |
| `input("What's your name? ")` | What is your first name? |