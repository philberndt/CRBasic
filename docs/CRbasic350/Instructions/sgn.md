# SGN (Sign Value)

The SGN function is used to find the sign value of a number.

## Syntax

SGN( number )

The example uses SGN to determine the sign of a number. A value is entered by the user in the variable UserEntry. The value stored in the Msg variable will indicate the sign of the entry.

```
'CR350 SeriesDatalogger

Public Msg,UserEntry'Declare variables
Dim Number

BeginProg
Scan (1,Sec,3,0)
Number = UserEntry'Get user input
Select CaseSGN( Number )'Evaluate Number
Case 0'Zero
Msg = 0
Case 1'Positive
Msg = 1
Case -1'Negative
Msg = -1
EndSelect
NextScan
EndProg
```

## Remarks

The SGN function returns an integer indicating the sign of a number.

The Number argument can be any valid numericexpression. The sign of Number argument determines the value returned by the SGN function.

If X > 0, then SGN( X ) = 1

If X = 0, then SGN( X ) = 0

If X < 0, then SGN( X ) = -1
