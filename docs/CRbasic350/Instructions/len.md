# Len (String Length)

The Len function is used to return the number of bytes in a string. The value returned is the number of bytes up to, but not including, the first null character.

## Syntax

_Variable_ = Len(StringVar)

In the following example program, the variable TestLenString is set to a value of 24 (since the input string for the Len instruction is comprised of 23 characters plus 1 null terminator).

```
Public TestString as String * 50
Public TestLenString

BeginProg
TestString="This is a string"
Scan (5,Sec,3,0)
TestLenString=Len(TestString)
NextScan
EndProg
```

## Parameter

# StringVar (String Variable)

The variable name of the string that will be evaluated using the Len function. The size of the StringVar must be set large enough to accommodate the size of the actual string (including the null-termination character), or the maximum value Len will return will be the size of the string (even if the actual string is larger). Note that strings are null-terminated; the null termination character counts as one of the characters in the string. If a Size is not specified when the StringVar is defined, the default string size is 24 (23 usable bytes and 1 null terminator).

Type: Variable defined as a String
