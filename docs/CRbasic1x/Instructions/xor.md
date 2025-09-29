# XOR (Logical Exclusion)

The XOR operator is used to perform a logical exclusion on two numbers.

## Syntax

result = Number1X OR Number2

The example sets the variable Msg based on the value of variables A, B, and C, assuming that no variable is a Null. If A = 10, B = 8, and C = 11, the left expression is True and the right expression is False. Because only one comparison expression is True, the XOR expression evaluates True.

```
Public A, B, C, Msg As Boolean'Declare variables.

BeginProg
A = 10: B = 8: C = 11'Assign values.
If A > BXORB > C Then'Evaluate expressions.
Msg = True
Else
Msg = False
EndIf
```

## Remarks

The XOR operator performs a bit-wise comparison of identically positioned bits in two numeric expressions and sets the corresponding bit in result according to the following table:

| If bit in Number 1 is | And bit in Number2 is | The result is |
| --------------------- | --------------------- | ------------- |
| 1                     | 1                     | 0             |
| 1                     | 0                     | 1             |
| 0                     | 1                     | 1             |
| 0                     | 0                     | 0             |

Bit-wise operations are performed on integers; floating point values will first be converted before the bit-wise operation is performed. Although XOR is a bit-wise operator, it is often used to test Boolean (True/False) conditions. Boolean values are implemented as Longs that are restricted to -1 (True) or 0 (False). Any non-zero number >= 1 will evaluate as true (a float value between .999 and 0 when converted to a Long is 0). Because XOR is a bit-wise operation, it is possible to XOR two non-zero numbers (for example, 2 and 4) and get a non-zero number. (XOR will only operate with two non-zero numbers and return 0 if the original numbers are equal.)

| If Number 1 is | And Number2 is                                                                                                                                                                    | The result is |
| -------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------- |
| -1             | -1                                                                                                                                                                                | 0             |
| -1             | NAN **Note:** Not a number. A data word indicating a measurement or processing error. Voltage overrange, SDI-12 sensor error, and undefined mathematical results can produce NAN. | NAN           |
| 0              | Any Number                                                                                                                                                                        | Number2       |
| 0              | NAN                                                                                                                                                                               | NAN           |

Expressions that are evaluated to a number can be used in place of one or both of the numbers.
