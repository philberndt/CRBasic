# StrComp (Compare Strings)

The StrComp function is used to compare two strings, generally for the purpose of determining if the strings are identical or to determine their sorting order. StrComp is case sensitive.

## Syntax

Variable =StrComp(String1,String2)

### Example #1

```
'Exploratory and demonstration

'use the table monitor to change the values
'of String1 and String2 to see how it affects
'the value of Result1
Public String1 As String = "apple"
Public String2 As String = "zebra"
Public Result1

Public Result2, Result3, Result4, Result5

BeginProg
Scan (1,Sec,3,0)

Result1 = StrComp(String1,String2)

'result2 = -1
' The letter "b" has a value of 98,
' and the letter "k" has a value of 107
' 98-107 = -9
Result2 =StrComp("bees","knees")

'result3 = 0
' The strings are identical
Result3 =StrComp("same","same")

'result4 = 1
' The strings are identical up to the last
' character. The character "2" has a value of 50.
' The character "1" has a value of "49".
' 50-49 = 1
Result4 =StrComp("2015-03-12","2015-03-11")

'result5 = -1
' StrComp is case sensitive. Therefore the first
' difference in the strings encountered are the
' the letters "C" and "c". "C" has a value
' of 67. "c" has a value of 99.
' 67-99 = -32
Result5 =StrComp("CASE","case")

NextScan
EndProg
```

### Example #2

```
'The following program shows how StrComp() can
'can be used in conjunction with loops to do an
'in-place ascending or descending sort of an array
'of strings.

Public Array1(5) As String * 23 = {"dog","brown","zebra","bee","apple"}
Public Array2(5) As String * 23 = {"dog","brown","zebra","bee","apple"}
Dim I, Done, tempStr As String * 23

BeginProg
Scan (1,Sec,3,0)

'Sort Array1 in Ascending Order
Do
Done = TRUE
For I = 1 To 4'for 1 to N-1
IfStrComp(Array1(I),Array1(I+1)) > 0 Then
'string2 should come before string1
'swap their positions
tempStr = Array1(I)
Array1(I) = Array1(I+1)
Array1(I+1) = tempStr
Done = FALSE'nope, need a clean pass through
EndIf
Next I
Loop Until Done

'Sort Array2 in Descending Order
Do
Done = TRUE
For I = 1 To 4'for 1 to N-1
IfStrComp(Array2(I),Array2(I+1)) < 0 Then
'swap their positions
tempStr = Array2(I)
Array2(I) = Array2(I+1)
Array2(I+1) = tempStr
Done = FALSE'nope, need a clean pass through
EndIf
Next I
Loop Until Done

NextScan
EndProg
```

### Example #3

```
' This example shows on method for creating a sub
' routine that sorts arrays of strings. Copy and paste
' this Subroutine into your program anywhere before
' EndProg and you can use / reuse it in your application.
' This example relies on the support for pointer notation.
' Support for pointers started in CRx000 OS version 28.

Sub SortStr(SrcPtr As Long,Swath As Long,Order As Long)
'This sub routine expects the following parameters:
' SrcPtr = a pointer to the array to be sorted
' Swath = the number of items to be sorted
' Order = 0: Ascending & 1: Descending
'Note: The strings in the source array can be
' a maximum of 255 characters long. This is governed
' by the declared length of str1 and str2.
Dim Done As Boolean
Dim I As Long
Dim ptr1 As Long, ptr2 As Long
Dim str1 As String * 255, str2 As String * 255
If Swath < 2 Then ExitSub 'nothing to sort
Do
Done = true'assume done
ptr1 = SrcPtr'get start
For I = ptr1 To (ptr1 + Swath - 2)'for 1 to N-1
str1 = !I'get str(N)
ptr2 = I+1'calc pointer to str(N+1)
str2 = !ptr2 'get str(N+1)
'if str1 > str2 and want ascending OR
'if str1 < str2 and want descending
If (StrComp(str1,str2) > 0 AND Order = 0) _
OR (StrComp(str1,str2) < 0 AND Order = 1) Then
'swap the position of the strings
!I = str2
!ptr2 = str1
'not done, unless there was only two strings
If Swath > 2 Then Done = false
EndIf
Next I
Loop Until Done
EndSub

Public Array1(5) As String * 23 = {"dog","brown","zebra","bee","apple"}
Public Array2(5) As String * 23 = {"dog","brown","zebra","bee","apple"}

BeginProg
Scan (1,Sec,0,0)

'use SortStr() to sort the array in ascending order
Call SortStr(@Array1,5,0)

'use SortStr() to sort the array in descending order
Call SortStr(@Array2,5,1)

NextScan
EndProg
```

## Remarks

The StrComp instruction is typically used to determine the sorting order of two strings. StrComp can also be used to simply determine if the strings are identical, but that task is more commonly accomplished with the logical expression of If String1 = String2 Then / EndIf .

Starting with the first character in each string, the characters in String2 are subtracted from the characters in String1 until the difference is non-zero or until the end of String2 is reached. The result of this instruction is an integer in the range of -255 to +255. If 0 is returned, the strings are identical. StrComp is case sensitive.

| If                             | StrComp Returns | Example                                |
| ------------------------------ | --------------- | -------------------------------------- |
| String1 sorts ahead of String2 | -1 to -255      | StrComp( bees , knees ) = -9           |
| String1 is equal to String2    | 0               | StrComp( same , same ) = 0             |
| String1 sorts after String2    | 1 to 255        | StrComp( 2015-03-12 , 2015-03-11 ) = 1 |

String variables can be declared as only one or two dimensions; for example, String(x) or String(x,y). To begin reading or modifying a string at a particular location into the string, enter the location as a third dimension; for example, String(x,y,n) where n is the desired character. For example, given an array of strings Str(10,10), Str(2,2,n) refers to n character in the (2,2) element of the array. Use Str(1,1,n) for a scalar variable and Str(x,1,n) for a one dimensional array element.

**NOTE:** String functions are case sensitive.[Uppercase](uppercase.md) or [Lowercase](lowercase.md) can be used to convert to all one case prior to processing the string if desired.
