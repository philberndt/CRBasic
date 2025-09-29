# IMP (Imply)

The IMP operator is used to perform a logical implication on two expressions.

## Syntax

Result = expression1IMPexpression2

```
'The following program shows the values returned by the IMP operator under different scenarios.
Public A = 10, B = 8, C = 6 , Result(4) as Boolean

BeginProg
Result(1) = A > B Imp B > C'returns true
Result(2) = B > A Imp B > C'returns true
Result(3) = A > B Imp C > A'returns false
Result(4) = B > A Imp C > B'returns true
EndProg
```

## Remarks

The following table illustrates how Result is determined:

| If expression1 is | And expression2 is | The Result is |
| ----------------- | ------------------ | ------------- |
| True              | True               | True          |
| True              | False              | False         |
| True              | Null               | Null          |
| False             | True               | True          |
| False             | False              | True          |
| False             | Null               | True          |
| Null              | True               | True          |
| Null              | False              | Null          |
| Null              | Null               | Null          |

The IMP operator performs a bitwise comparison of identically positioned bits in two numeric expressions and sets the corresponding bit in result according to the following table:

| If Bit in expression1 is | And Bit in expression2 is | The Result is |
| ------------------------ | ------------------------- | ------------- |
| 0                        | 0                         | 1             |
| 0                        | 1                         | 1             |
| 1                        | 0                         | 0             |
| 1                        | 1                         | 1             |
