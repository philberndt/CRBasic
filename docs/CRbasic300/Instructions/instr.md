# InStr (Find In String)

The InStr function is used to find the location of a string within a string.

## Syntax

Variable =InStr(Start,SearchString,FilterString,SearchOption)

In the following program example, the Instr instruction is used to find the position of the text "direction:" in a string (SearchString). The result is stored in the variable Start1.

```
Public SearchString AS STRING * 80
Public FilterString1 AS STRING * 20
Public Start1

BeginProg
SearchString = "maximum wind speed: 100 mph; direction: 263 degrees; Site 11 Cedar Mountain."
FilterString1 = "direction:"
Scan (1,Sec,0,0)
Start1 =InStr(1,SearchString,FilterString1,2)
NextScan
EndProg
```

## Remarks

This function returns the integer position of the first occurrence of the FilterString parameter. If the FilterString is not found, the function returns 0.

## Parameters

# Start

Specifies where in the SearchString to begin. A 1 indicates the first character in the string.

Type: Integer

# SearchString (Search String)

The string to evaluate.

Type: String or variable

# FilterString (Filter String)

The string to look for in the SearchString.

Type: String

For a FilterString using non-printable ASCII characters, use the [CHR](chr.md) function and the appropriate ASCII code.

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

String variables can be declared as only one or two dimensions; for example, String(x) or String(x,y). To begin reading or modifying a string at a particular location into the string, enter the location as a third dimension; for example, String(x,y,n) where n is the desired character. For example, given an array of strings Str(10,10), Str(2,2,n) refers to n character in the (2,2) element of the array. Use Str(1,1,n) for a scalar variable and Str(x,1,n) for a one dimensional array element.

**WARNING:** String functions are case sensitive. Uppercase or lowercase can be used to convert to all one case prior to processing the string if desired.
