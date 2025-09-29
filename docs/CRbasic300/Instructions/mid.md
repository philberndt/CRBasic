# Mid (Substring within a String)

The Mid instruction is used to return a substring that is within a string.

## Syntax

String =Mid(SearchString,Start,Length)

In the following example program, the Mid instruction is used to store a portion of a string in a new variable. InStr is used to find the beginning integer position of the text "direction:". Then Mid is used to return the string beginning at this point and ending 22 characters later. The result is that "direction: 263 degrees" is stored in the Midstring1 variable.

```
Public SearchString AS STRING * 80
Public FilterString1 AS STRING * 20
Public Start1
Public MidString1 AS STRING * 20
Const MaxLength1 = 22

BeginProg
Scan (1,Sec,0,0)
SearchString = "maximum wind speed: 100 mph; direction: 263 degrees; Site 11 Cedar Mountain."
FilterString1 = "direction:"
Start1 = InStr (1,SearchString,FilterString1,2)
MidString1 =Mid(SearchString, Start1, MaxLength1)
NextScan
EndProg
```

## Remarks

The Start and Length parameters are used to determine which part of the SearchString is returned. Regardless of the value of the Length parameter, the returned string is no longer than the original string.

## Parameters

# SearchString (Search String)

The string to evaluate.

Type: String or variable

# Start

Specifies where in the SearchString to begin. A 1 indicates the first character in the string.

Type: Integer

If Start is beyond the current length of the string (first null), a null string is returned.

# Length

Specifies the maximum number of characters returned from the original string.

Type: Integer

String variables can be declared as only one or two dimensions; for example, String(x) or String(x,y). To begin reading or modifying a string at a particular location into the string, enter the location as a third dimension; for example, String(x,y,n) where n is the desired character. For example, given an array of strings Str(10,10), Str(2,2,n) refers to n character in the (2,2) element of the array. Use Str(1,1,n) for a scalar variable and Str(x,1,n) for a one dimensional array element.

**NOTE:** String functions are case sensitive. Uppercase or lowercase can be used to convert to all one case prior to processing the string if desired.
