# C# sheet

## Special Operators

| Operator | Beschrijving                                                                                                                                           |
| :------: | :----------------------------------------------------------------------------------------------------------------------------------------------------- |
|   `?:`   | **Ternary operator**: Verkorte schrijfwijze voor if-else structuur `return (a==b) ? c : d` als `a==b` true geeft dan wordt `c` teruggegeven anders `d` |
|   `??`   | **Null-coalescing operator**: Retourneert de linkerkant van de operand als niet null, anders de rechterkant `int y = x ?? -1`                          |
|   `?.`   | **Null conditional operator**: Test op null alvorens een member access te doen `int? length = customers?.Length`                                       |
