# SearchOption (Search Option)

A code used to help define the method of searching.

| Code | Description                                                                                                |
| ---- | ---------------------------------------------------------------------------------------------------------- |
| 0    | NUMERIC - Numerics in the SearchString (FilterString is ignored).                                          |
| 1    | NON-NUMERIC - Non-numerics (FilterString is ignored).                                                      |
| 2    | SEARCHSTRING - Each FilterStrings in SearchString.                                                         |
| 3    | SEARCHCHARS - Each occurrence of any character that is in FilterString.                                    |
| 4    | HEADERFILTER - Strings succeeding FilterString.                                                            |
| 6    | HEADERFILTERCHARS - Strings succeeding any character in the FilterString char list.                        |
| 8    | NUMERICHEX - Hexadecimal numerics in the SearchString (FilterString is ignored).                           |
| 9    | ReverseCS first occurrence of the filter string, searching from the end of the string. Case sensitive.     |
| 10   | ReverseIS - first occurrence of the filter string, searching from the end of the string. Case insensitive. |

Add 100 to any of the non-numeric options above to parse a string that includes quotes, with the quotes being omitted in the result.

Type: Constant
