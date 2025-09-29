# Right (Characters from Right Side)

The Right function returns a substring that is a defined number of characters from the right side of the original string.

## Syntax

_variable_ = Right(SearchString,NumChars)

The following example program shows the use of the Left and Right string functions. Each function has a NumChars parameter of 10, which returns 10 characters from the left or right of the string. Using the original string, the Left function returns "The quick " and the Right function returns " the fence".

The InputString can be changed, so you can see the effect of different string entries when processed by the functions. Results can be viewed easily by collecting the data and viewing the file.

```
Public StringRight as String * 50, StringLeft as String * 50
Public InputString AS String * 50

DataTable (StringTab,True,-1)
Sample (1,StringLeft,String)
Sample (1,StringRight,String)
EndTable

BeginProg
InputString="The quick brown fox jumped over the fence"
Scan (1,Sec,3,0)
StringLeft =Left(InputString,10)
StringRight =Right(InputString,10)
CallTable (StringTab)
NextScan
EndProg
```

## Parameters

# SearchString (Search String)

The string to evaluate.

Type: String or variable

# NumChars (Number of Characters)

Specifies the number of characters from the left or right side of the string to return.

Type: Variable or Constant
