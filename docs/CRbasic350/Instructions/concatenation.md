# Concatenation &

The & operator is used to concatenate strings.

## Syntax

String1&String2&String3

The following example program shows the use of the & string concatenation operator. MyString will result in the string, This is my string and MyOtherString will result in This is a new string .

```
Public MyString as String * 30
Public MyOtherString as String * 30
Public String1 as string * 10
Public String2 as string * 10
Public String3 as string * 10

BeginProg
String1 = "This is"
String2 = " a new"
String3 = " string"

Scan (1,Sec,3,0)
MyString="This"&" is"&" my"&" string"
MyOtherString=String1&string2&string3
NextScan
EndProg
```

## Remarks

The values being concatenated must be strings (integers are converted to strings). If you need to concatenate strings and variables, use the + operator. Note that when adding values, once a string is encountered, all subsequent operands will first be converted to a string before the + operation is performed. When working with strings the & operator can be safer to use than + because there is no danger of a value being converted from a string to an integer.

From each source string, the operator will use all characters up to the first null character. Null characters will not be included in the resulting string, except for the terminating character.
