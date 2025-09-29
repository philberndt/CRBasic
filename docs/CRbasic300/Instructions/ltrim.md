# LTrim (Trim Leading Spaces)

The LTrim function returns a copy of a string with no leading spaces.

## Syntax

_variable_ = LTrim(TrimString)

The following example program shows the use of the trim functions to remove the leading and/or trailing spaces from a string.

The InputString is initialized as a text string with 5 leading spaces and 5 trailing spaces. However, the InputString can be changed, so you can see the effect of different string entries when processed by the functions. Results can be viewed easily by collecting the data and viewing the file.

```
Public TrimRight as String * 50, TrimLeft as String * 50, TrimAll as String * 50
Public InputString AS String * 50

DataTable (TrimTab,True,-1)
Sample (1,TrimLeft,String)
Sample (1,TrimRight,String)
Sample (1,TrimAll,String)
EndTable

BeginProg
InputString= " This is a test "
Scan (1,Sec,3,0)
TrimLeft =LTrim(InputString)
TrimRight =RTrim(InputString)
TrimAll =Trim(InputString)
CallTable (TrimTab)
NextScan
EndProg
```

## Remarks

The TrimString parameter is the string that should be stripped of leading spaces.

To trim trailing spaces only, use [RTrim](rtrim.md). To trim both leading and trailing spaces, use [Trim](trim.md).
