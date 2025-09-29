# AND

The AND operator is used to perform a logical conjunction of two numbers.

## Syntax

result = Number1 AND Number2

The example assigns a value to Msg that depends on the value of variables A, B, and C, assuming that no variable is a Null. If A = 10, B = 8, and C = 6, both expressions evaluate True. Because both expressions are True, the AND expression is also True.

```
'CR350 SeriesDatalogger

Public A, B, C, Msg AS Boolean'Declare variables.

BeginProg
A = 10: B = 8: C = 6'Assign values.
Scan (1,Sec,3,0)
If A > BANDB > C Then'Evaluate expressions.
Msg = True
Else
Msg = False
EndIf
NextScan
EndProg
```

The following program is an example of using AND to check individual bits in a Long, and using AND to mask bits.

```
Public SourceInteger As Long, MaskedInteger As Long
Public Bit0 As Boolean, Bit1 As Boolean

BeginProg
Scan (1,Sec,0,0)
SourceInteger = &b1100010
'Checking individual bits with AND
Bit0 = SourceIntegerAND1' 2^0 &b1
Bit1 = SourceIntegerAND2' 2^1 &b1

'Using a bit mask with AND to return a portion of the source
MaskedInteger = SourceIntegerAND&b0111111
NextScan
EndProg
```

## Remarks

The AND operator performs a bit-wise comparison of identically positioned bits in two numeric expressions and sets the corresponding bit in result according to the following truth table:

| If bit in Number1 is | And bit in Number2 is | The result is |
| -------------------- | --------------------- | ------------- |
| 0                    | 0                     | 0             |
| 0                    | 1                     | 0             |
| 1                    | 0                     | 0             |
| 1                    | 1                     | 1             |

Bit-wise operations are performed on integers; floating point values will first be converted before the bit-wise operation is performed. Although AND is a bit-wise operator, it is often used to test Boolean (True/False) conditions. Boolean values are implemented as Longs that are restricted to -1 (True) or 0 (False). Any non-zero number >= 1 will evaluate as true (a float value between .999 and 0 when converted to a Long is 0). Because AND is a bit-wise operation, it is possible to AND two non-zero numbers (for example, 2 and 4) and get 0. The binary representation of -1 has all bits equal 1. That is, any number AND -1 returns the original number. That is why the predefined constant True = -1.

If, and only if, both expressions evaluate True, result is True. If either expression evaluates False, then result is False. The following table illustrates how result is determined:

| If Number 1 is | AND Number2 is                                                                                                                                                                    | The result is |
| -------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------- |
| -1             | Any number                                                                                                                                                                        | Number2       |
| -1             | NAN **Note:** Not a number. A data word indicating a measurement or processing error. Voltage overrange, SDI-12 sensor error, and undefined mathematical results can produce NAN. | NAN           |
| 0              | Any number                                                                                                                                                                        | 0             |
| 0              | NAN                                                                                                                                                                               | NAN           |

Expressions can be used in place of one or both of the numbers. Comparison expressions evaluate as True (-1) or False (0).
