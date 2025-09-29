# ASCII

The ASCII function is used to return the ASCII value of a character in a string.

## Syntax

_Variable_ = ASCII(ASCIIString(1,1,X) )

The following example program uses the ASCII function to process each character in a string named ACIIString. The result is stored in a data table. The [Mid](mid.md) instruction is used to return the actual character being read. The table is used as a visual check to better understand the results of the ASCII function.

Note that the Scan count is set to 1, so the program runs once and stops.

```
Public ASCIIString as String * 55'Source string for the ASCII function
Public ASCIIChr'Variable to step thru each char in the string
PublicReturnString 'Destination variable for ASCII function
Public CheckString As String'Destination variable for MID function

DataTable (ASCIITab,True,-1)
Sample (1,ReturnString,FP2)
Sample (1,CheckString,String)
EndTable

BeginProg
Scan (1,Sec,3,1)
ASCIIString="abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ"
For ASCIIChr = 1 to 52
ReturnString=ASCII(ASCIIString(1,1,ASCIIChr))
CheckString=Mid(ASCIIString,ASCIIChr,1)
CallTable (ASCIITab)
Next ASCIIChr
```

NextScan
EndProg

```

## Remarks


Variables that are declared as strings can have only two dimensions. If a third dimension is used for a string, it represents the character within the string. Therefore, in the previous syntax example, X is a value that represents the position of the character in the string that you want returned. If your string is ABCDEFG and you want the ASCII value returned of D, you would use the number 4 for X to return that value.


## Parameter



# ASCIIString


The string that should be processed with the function.

Variables that are declared as strings can have only two dimensions. If a third dimension is used for a string, it represents the character within the string. Therefore, to specify the character for which to return the value, use ASCIIString(1,1,X) where X is a value that represents the position of the character in the string that you want returned. If your string is ABCDEFG and you want the ASCII value returned of D, you would use the number 4 for X to return that value.

Type: Variable declared as a String
```
