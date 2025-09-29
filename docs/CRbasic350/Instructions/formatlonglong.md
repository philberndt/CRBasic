# FormatLongLong (Convert a 64-bit Long Value)

The FormatLongLong function is used to convert a 64-bit long integer into a decimal value in the format of a string variable.

## Syntax

```
FormatLongLong
```

(LongLongVar(1))

The following code is a test program showing the use of the FormatLongLong function. Type two values (high-order bits, low-order bits) into the Num() array in your datalogger support software's numeric display. Results of the input are saved in the Store variable.

```
Public Num(2) As Long
Public Store As String

BeginProg
Scan (1,Sec,3,0)
Store=FormatLongLong(Num(1))
NextScan
EndProg
```

## Remarks

The source for this function (LongLongVar) is assumed to be two adjacent variables formatted as Long. The first variable contains the high-order 32-bits and; the second contains the low-order 32-bits.
