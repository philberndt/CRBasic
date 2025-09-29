# Replace (Find/Replace)

The Replace function is used to search a string for a substring, and replace that substring with a different string.

## Syntax

_variable_ = Replace(SearchString,SubString,ReplaceString)

The following program shows the use of the Replace function for manipulating strings. Any occurrence of the letter "e" in the input string (StringI) is changed to a "9".

The Input string can be changed, so you can see the effect of different string entries when processed by the function. Results can be viewed easily by collecting the data and viewing the file.

```
Public StringI as String * 50, StringR As String * 50

DataTable (StringTab,True,-1)
Sample (1,StringI,String)
Sample (1,StringR,String)
EndTable

BeginProg
StringI = "This is a test string a b c d e"
Scan (1,Sec,3,0)
StringR =Replace(StringI,"e","9")
CallTable (StringTab)
NextScan
EndProg
```

## Remarks

The new string resulting from this function is stored in the _variable_.

## Parameters

# SearchString (Search String)

The string to evaluate.

Type: String or variable

# SubString (Part of String)

The portion of the string in the original string that will be replaced.

Type: String or String Variable

# ReplaceString (Replace String)

The string that should be used to replace the SubString.

Type: String or String Variable

**WARNING:** String functions are case sensitive.[Uppercase](uppercase.md) or [lowercase](lowercase.md) can be used to convert to all one case prior to processing the string if desired.
