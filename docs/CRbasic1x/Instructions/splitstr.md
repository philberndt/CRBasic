# SplitStr (Split Strings)

The SplitStr instruction is used to split out one or more strings or numeric variables from an existing string.

## Syntax

SplitStr(SplitResult,SearchString,FilterString,NumSplit,SplitOption)

### Example #1

In the following example, a string is split into 5 different arrays using the SplitStr instruction. The "," is used to split the search string.

```
Public SearchString As String * 50
Public SplitResult(5) As String * 10

BeginProg
Scan (1,Sec,3,0)
SearchString="String1,String2,String3,String4,String5"
SplitStr(SplitResult(1),SearchString,",",5,5)
NextScan
EndProg
```

### Example #2

In this example, numeric values in the SearchString are returned; other characters are discarded.

```
Public SearchString As String * 65
Public ResultString(5) As String * 10

BeginProg
Scan (1,Sec,3,0)
SearchString="String123ABCString234DEFString345GHIString456JKLString678MNO"
SplitStr(ResultString(1),SearchString,",",5,0)
NextScan
EndProg
```

The result for each variable would be:

- ResultString(1)=123

- ResultString(2)=234

- ResultString(3)=345

- ResultString(4)=456

- ResultString(5)=678

### Example #3

In this example, non-numeric values in the SearchString are returned; numeric values are discarded.

```
Public SearchString As String * 50
Public ResultString(6) As String * 10

BeginProg
Scan (1,Sec,3,0)
SearchString="String1ABCString2DEFString3GHIString4JKLString5MNO"
SplitStr(ResultString(1),SearchString,",",6,1)
NextScan
EndProg
```

The result for each variable would be:

- ResultString(1)=String

- ResultString(2)=ABCString

- ResultString(3)=DEFString

- ResultString(4)=GHIString

- ResultString(5)=JKLString

- ResultString(6)=MNO

### Example #4

In this example, a string is split based on either a space (CHR(32)) or a colon (CHR(58)).

```
Public SearchString(2) As String * 30
Public ResultString(5) As String * 20

BeginProg
SearchString() = "013CAMPBELLL LR4 2.0 SN:12345"
Scan (1,Sec,3,0)

SplitStr(ResultString(),SearchString(),CHR(32)& CHR(58),5,7)

NextScan
EndProg
```

The result for each variable would be:

- ResultString(1)=013CAMPBELLL

- ResultString(2)=LR4

- ResultString(3)=2.0

- ResultString(4)=SN

- ResultString(5)=12345

### Example #5

The following example program demonstrates using a structure named Sensor1 to store the split string result. See [StructureType()](../parameters/structuretypename.md) for more information on structures.

```
StructureType (Values)'Declare structure type Values
val_Float As Float
val_Long As Long
val_String As String
EndStructureType

Public Sensor1 As Values'Declare a public structure named Sensor1 as structure type Values
Public Measurements As String * 50

'Main Program
BeginProg
Measurements = "1.23,123,ABC"
Scan (1,Sec,0,0)
SplitStr(Sensor1,Measurements,",",3,5)'Store split string result to Sensor1 structure
NextScan
EndProg
```

In a Data Monitor, the Sensor1 structure would look like this:

## Remarks

The splitting can be based on fixed delimiters (for example, commas) or semi-automatic (for example, reading numeric variables out of a string of mixed types as might be returned by a sensor). The FilterString and SplitOption help to define the array returned by the SplitStr instruction.

## Parameters

# SplitResult (Result of SplitStr)

An array or structure in which the split string will be stored.

Type: Variable Array or Structure

# SearchString (Search String)

The string to evaluate.

Type: String or variable

# FilterString (Filter String)

Provides a filter for the string(s) to be returned. For a FilterString using non-printable ASCII characters, use the [CHR](chr.md) function and the appropriate ASCII code. For a full list of ASCII codes, see [ASCII Codes and Characters](../Info/CodesChar.md).

Type: String or Variable

# NumSplit (Maximum Number of Strings)

The maximum number of strings or values returned by the instruction.

Type: Constant

# SplitOption (Split Option)

A code specifying the method used to split the string.

| Code | Description                                                                                                                                                                                                                                                                                                                                                                                      |
| ---- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| 0    | NUMERIC Numeric values are split out of the SearchString and stored into the array. Non-numeric characters are discarded. Delimiters are any characters but + - . 0 1 2 3 4 5 6 7 8 9 0 E). With this option, FilterString is ignored.                                                                                                                                                           |
| 1    | NON-NUMERIC Non-numeric sub-strings (text strings) are split out of the SearchString and stored into the array. Numeric characters are discarded. Delimiters are . 0 1 2 3 4 5 6 7 8 9 0 E). With this option, FilterString is ignored.                                                                                                                                                          |
| 2    | SEARCHSTRING - SearchString is split and stored into the array based upon the occurrence of the entire FilterString.                                                                                                                                                                                                                                                                             |
| 3    | SEARCHCHARS - SearchString is split and stored into the array based upon each occurrence of any character that is in FilterString.                                                                                                                                                                                                                                                               |
| 4    | HEADERFILTER - Any string succeeding FilterString is returned in SplitResult.                                                                                                                                                                                                                                                                                                                    |
| 5    | FOOTERFILTER -The number of values specified are parsed from the SearchString using the FilterString, or until the end of the SearchString is reached.                                                                                                                                                                                                                                           |
| 6    | HEADERFILTERCHARS - Strings succeeding any character in the FilterString char list are returned in SplitResult. Redundant delimiters are treated as a single delimiter; for example, a string of AA BB CC with a Header Filter of (space) will be returned as AA BB CC . In addition, multiple delimiters can be specified; for example, :; will split a string based on the colon or semicolon. |
| 7    | FOOTERFILTERCHARS - Strings preceding any character in the FilterString char list are returned in SplitResult. Redundant delimiters are treated as a single delimiter. In addition, multiple delimiters can be specified; for example, :; will split a string based on the colon or semicolon.                                                                                                   |
| 8    | NUMERICHEX - SearchString is split and stored in the array based upon the occurrence of hexadecimal numerics in the string (delimiters are any character but 0 1 2 3 4 5 6 7 8 9 0 A B C D E F). The hexadecimal value is stored in the array. With this option, FilterString is ignored.                                                                                                        |
| 1x   | Where X is one of the options above, right justify the resultant array, filling vacant elements withNAN **Note:** Not a number. A data word indicating a measurement or processing error. Voltage overrange, SDI-12 sensor error, and undefined mathematical results can produce NAN. (if numeric) or a NULL string if a string.                                                                 |

For option 5, if a null string exists between two filter strings, a null string will be returned. For option 7, any null strings will be ignored.

Add 100 to any of the non-numeric options above to parse a string that includes quotes, with the quotes being omitted in the result.

String variables can be declared as only one or two dimensions; for example, String(x) or String(x,y). To begin reading or modifying a string at a particular location into the string, enter the location as a third dimension; for example, String(x,y,n) where n is the desired character. For example, given an array of strings Str(10,10), Str(2,2,n) refers to n character in the (2,2) element of the array. Use Str(1,1,n) for a scalar variable and Str(x,1,n) for a one dimensional array element.

The [Mid](mid.md) instruction can also be used to split strings. It may be useful in cases where SplitStr will not work.

**NOTE:** String functions are case sensitive.[Uppercase](uppercase.md) or [lowercase](lowercase.md) can be used to convert to all one case prior to processing the string if desired.
