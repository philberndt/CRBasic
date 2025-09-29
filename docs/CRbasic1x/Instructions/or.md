# OR

The OR operator is used to perform a logical disjunction on two numbers.

## Syntax

result = Number1 OR Number2

The example sets the variable Msg true, based on the values of variables A, B, and C. If A = 10, B = 8, and C = 11, the left expression is True and the right expression is False. Because at least one comparison expression is True, the OR expression evaluates True.

```
'Declare variables.
Public A, B, C, Msg As Boolean

BeginProg
A = 10: B = 8: C = 11'Assign values.
If A > BORB > C Then'Evaluate expressions.
Msg = True
Else
Msg = False
EndIf
EndProg
```

## Remarks

The OR operator performs a bit-wise comparison of identically positioned bits in two numeric expressions and sets the corresponding bit in Result according to the following truth table:

| If bit in Number1 is | And bit in Number2 is | The result is |
| -------------------- | --------------------- | ------------- |
| 0                    | 0                     | 0             |
| 0                    | 1                     | 1             |
| 1                    | 0                     | 1             |
| 1                    | 1                     | 1             |

Bit-wise operations are performed on integers; floating point values will first be converted before the bit-wise operation is performed. Although OR is a bit-wise operator, it is often used to testBoolean

**Note:** Data type used to represent conditions or hardware that have only two states (true or false) such as flags and control ports.

(True/False) conditions. Boolean values are implemented as Longs that are restricted to -1 (True) or 0 (False). Any non-zero number >= 1 will evaluate as true (a float value between .999 and 0 when converted to a Long is 0). The predefined constant True = -1. The binary representation of -1 has all bits equal 1. Thus, any number OR -1 returns -1. Any number [AND](and.md)-1 returns the original number.

| If Number1 is | And then Number2 is                                                                                                                                                               | The result is |
| ------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------- |
| -1            | Any number                                                                                                                                                                        | -1            |
| -1            | NAN **Note:** Not a number. A data word indicating a measurement or processing error. Voltage overrange, SDI-12 sensor error, and undefined mathematical results can produce NAN. | NAN           |
| 0             | Any number                                                                                                                                                                        | Number2       |
| 0             | NAN                                                                                                                                                                               | NAN           |

Expressions that are evaluated to a number can be used in place of one or both of the numbers.
