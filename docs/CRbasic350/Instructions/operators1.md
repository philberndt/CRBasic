# Operators

Math operators that can be used in datalogger programs:

|       |                                                    |
| ----- | -------------------------------------------------- |
| ^     | Raise to power (or use [PWR](pwr.md) function)     |
| +     | Add, or string [concatenation](concatenation.md)   |
| -     | Subtract                                           |
| \*    | Multiply                                           |
| /     | Divide                                             |
| =     | Equal                                              |
| <>    | Not equal                                          |
| >     | Greater than                                       |
| <     | Less than                                          |
| >=    | Greater than or equal                              |
| <=    | Less than or equal                                 |
| >>    | [Bit shift](bitshiftoperators.md) operation, right |
| <<    | Bit shift operation, left                          |
| &     | String [concatenation](concatenation.md)           |
| \     | Integer division                                   |
| \*=   | [Compound](compoundoperators.md) multiplication    |
| +=    | Compound addition                                  |
| -=    | Compound subtraction                               |
| /=    | Compound division                                  |
| \=    | Compound integer division                          |
| ^=    | Compound exponent                                  |
| &=    | Compound string concatenation                      |
| AND   | Logical conjunction                                |
| IMP   | Logical implication                                |
| INTDV | Integer division                                   |
| MOD   | Modulo divide                                      |
| NOT   | Logical negation                                   |
| OR    | Logical disjunction                                |
| XOR   | Logical exclusion                                  |
| @     | [Pointer variable](../Info/pointervariable.md)     |
| !     | Dereference operator                               |

**NOTE:** When using + to combine integers and strings, once a string is encountered, all values thereafter are evaluated as strings.

String evaluation is case sensitive.

## Precedence/Order of Operation

(^)

(+ [positive], [negative], NOT)

(\*, /, INTDV, MOD)

(+ [addition], [subtraction],string concatenation [+,&])

(=, <>, <, <=, >, >=, Is)

(<<, >>, And, Or, Xor, Eqv, Imp)

Operators with the same precedence are evaluated in the order they are written inside the expression.

## Long Data Types

Equations are evaluated from left to right, taking note of any precedence rules. If the variables or constants used in an equation are of type Long or are integers and the functions used will give integer results (for example INTDV) the result of each stage of the evaluation will be a Long integer. However, if part of the equation has a real variable or constant in it or a function that results in a floating point value, then the rest of the equation will be evaluated using real math. This can be critical if trying to use Long integers math to retain numerical resolution beyond the limit of real variables (24 bits).

## Truth Table for Common Logical Operators

| a   | b   | NOT a | a and b | a OR b | a XOR b | a IMP b | a EQV b |
| --- | --- | ----- | ------- | ------ | ------- | ------- | ------- |
| T   | T   | F     | T       | T      | F       | T       | T       |
| T   | F   | F     | T       | T      | F       | F       |
| F   | T   | T     | F       | T      | T       | T       | F       |
| F   | F   | F     | F       | F      | T       | T       |

**NOTE:** CRBasic order of operation may vary from some implementations of BASIC. When in doubt always use parentheses to denote order of operation.
