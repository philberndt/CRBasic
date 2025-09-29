# Multi-dimensional Arrays

Two- and three-dimensional arrays can be defined in a datalogger program. A two-dimensional array named Temp would be defined by Public Temp(X,Y) and a three-dimensional array by Public Temp(X,Y,Z) (the Dim statement can also be used; for example, Dim Temp(X,Y)).

If a variable is declared as a String, it can be declared as only one or two dimensions. The third dimension is used to access a character within the string (not all dataloggers support data types).

Within the datalogger program, multi-dimensional arrays can be referenced using the multi-dimensional form, or by using an index into the array. When doing Reps in a measurement or output instruction, the index form can be more convenient than the multi-dimensional form.

In general, an array is arranged in memory by the concept of beginning with the least significant dimension (right most) counting up to the most significant dimension (left most). The following table shows the variables created by the statement Public Temp(3,3,3) and the index value associated with each variable.

|                  |                  |                  |
| ---------------- | ---------------- | ---------------- |
| 1 `Temp(1,1,1)`  | 2 `Temp(1,1,2)`  | 3 `Temp(1,1,3)`  |
| 4 `Temp(1,2,1)`  | 5 `Temp(1,2,2)`  | 6 `Temp(1,2,3)`  |
| 7 `Temp(1,3,1)`  | 8 `Temp(1,3,2)`  | 9 `Temp(1,3,3)`  |
| 10 `Temp(2,1,1)` | 11 `Temp(2,1,2)` | 12 `Temp(2,1,3)` |
| 13 `Temp(2,2,1)` | 14 `Temp(2,2,2)` | 15 `Temp(2,2,3)` |
| 16 `Temp(2,3,1)` | 17 `Temp(2,3,2)` | 18 `Temp(2,3,3)` |
| 19 `Temp(3,1,1)` | 20 `Temp(3,1,2)` | 21 `Temp(3,1,3)` |
| 22 `Temp(3,2,1)` | 23 `Temp(3,2,2)` | 24 `Temp(3,2,3)` |
| 25 `Temp(3,3,1)` | 26 `Temp(3,3,2)` | 27 `Temp(3,3,3)` |

## Dimensional Array Operations

CRBasic supports syntax for operating on a single dimension in a multi-dimensional array in a manner, that, in the background, is similar to acting on the array using a For/Next loop. Potential uses for this syntax include:

- Initializing an array.

- Copying a dimension of an array into a new location.

- Scaling the values for a dimension in an array.

- Performing a mathematical or logical operation for each element in a dimension using scalar or similarly-located elements in a different array and dimension.

Following are syntax rules and behaviors for operating on the dimension of an array. Given the array Array(A,B,C):

- The () pair must always be present; i.e., reference the array as Array() or Array(A,B,C)().

- Only one dimension of the array can be operated on at a time.

- To select the dimension, negate the element index. For instance, to operate on the dimension B, the syntax would be Array(A,-B,C)(). The array is accessed from the specified starting point to the end of the dimension. If no element is negated, or if indices are not specified, the default is the least significant dimension. Operations will not cross from one dimension into another.

- The offset into the dimension being accessed is given by A, B, and C in Array(A,B,C)().

- If the Array is referenced as Array(), then the starting point is assumed Array(1,1,1) and the least significant dimension is accessed.

```
'Example: Fill Array Dimension

Public A(3)
Public B(3,2)
Public C(4,3,2)
Public Da(3,2) = {1,1,1,1,1,1}
Public Db(3,2)
Public DMultiplier(3) = {10,100,1000}
Public DOffset(3) = {1,2,3}
BeginProg
Scan (1,Sec,0,0)

A() = 1'set all elements of 1D array or first dimension to 1

B(1,-1)() = 100'set B(1,1) and B(1,2) to 100
B(-2,1)() = 200'set B(2,1) and B(3,1) to 200
B(-2,2)() = 300'set B(2,2) and B(3,2) to 300

C(1,-1,1)() = A()'copy A(1), A(2), and A(3) into C(1,1,1), C(1,2,1), and C(1,3,1), respectively
C(2,-1,1)() = A() * 1.8 + 32'scale and then copy A(1), A(2), and A(3) into C(2,1,1), C(2,2,1), and C(2,3,1), respectively

'scale the first column of Da by corresponding multiplier and offset
'copy the result into the first column of Db
'then set second column of Db to NAN
Db(-1,1)() = Da(-1,1)() * DMultiplier() + DOffset()
Db(-1,2)() = NAN

NextScan
EndProg
```
