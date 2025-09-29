# Erase

The Erase instruction is used to quickly clear bytes in a variable or variable array.

## Syntax

Erase(EraseVar)

The following simple program shows how to use Erase to clear a string variable. Set the aliased variable WriteMessage to true to write a simple message into a string, then set the variable EraseMessage to true to erase the variable.

```
Public Message As String * 100, Flag(2) As Boolean
Alias Flag(1) = WriteMessage
Alias Flag(2) = EraseMessage

BeginProg

Scan (1,Sec,3,0)
If Flag(1) Then
Message="The date and time are " + Public.Timestamp
Flag(1)=False
EndIf

If Flag(2) Then
Erase(Message)
Flag(2)=False
EndIf

NextScan
EndProg
```

## Remarks

The Erase instruction sets all bytes of a variable in memory to 0, from the starting point (specified by EraseVar) to the end of the variable or variable array. It can be used to clear variables of any data type (including strings), and the variable can be a scalar or an array. The exception is that Erase cannot be used on dereferenced pointers.

The EraseVar parameter is the name of the variable in the program to be cleared. In its simplest form, you can clear all bytes in a variable/variable array, or you can specify a starting point and the variable/array is cleared to the end.

Consider the following:

Public MyVar(5)

Erase (MyVar()) will set all bytes to 0 in the entire variable array

Erase (MyVar(3) will set all bytes to 0 in MyVar(3), MyVar(4), and MyVar(5)

Erase (MyVar(1,1,3) will set all but the first two bytes to 0 in the MyVar array (the operation begins at position 3)
