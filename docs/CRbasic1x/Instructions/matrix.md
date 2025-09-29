# Matrix (Matrix Math)

The Matrix instruction performs matrix math with two-dimensional arrays (matrices).

## Syntax

Matrix(Option,A,B,Result)

Following are two example programs. The first program demonstrates basic use of the Matrix instruction with 4 x 4 and 3x3 matrices. The second example uses the Matrix instruction to transfer air-flow speeds to a 3D sonic anemometer coordinate system.

### Example #1

```
'Declare Variables
Dim Xi(4,4) = {5, -2, 2, 7, 1, 0, 0, 3, -3, 1, 5, 0, 3, -1, -9, 4}
Dim Xm(3,3) = {1,2,0,0,1,1,2,0,1}, Ym(3,3) = {1,1,2,2,1,1,1,2,1}
Dim X(3,3) = {1,2,3,4,5,6,7,8,9}
Public Add(3,3), Subtract(3,3), Multiply(3,3), Transpose(3,3), Inverse(4,4)

'Main Program
BeginProg
Scan (1,Sec,0,1)
Matrix(1,X,X,Add)
Matrix(2,X,X,Subtract)
Matrix(3,Xm,Ym,Multiply)
Matrix(4,X,0,Transpose)
Matrix(5,Xi,0,Inverse)
NextScan
EndProg
```

### Example #2

```
Public Ptemp, Batt_volt
'Real matrixes for IRGASON SN 1192
'1. Transfer the air flow speeds along each of sonic paths
'to 3-dimensional (3D) right-handed sonic anemometer coordinate system
Public A_Matrix(3,3) = {0.019905, 1.152260, -1.183869, 1.336701, -0.685270, -0.643872, 0.379722, 0.398294, 0.374232}
'2. Transfer the air flow speeds in the 3D right-handed sonic anemometer coordinate system
'to the flow speed along each of sonic paths
Public A_matrix_inversion(3, 3) = {0.00000, 0.502508, 0.864573,0.414551,-0.254382,0.873749,-0.441205,-0.239141,0.864956}
'Matrix used To verify A_matrix() + A_matrix()
Public Result_A_plus_A (3, 3)
'Matrix used to verify A_matrix() - A_matrix(). It is a zero matrix
Public Result_0 (3,3)
'Matrix used to verify A_matrix() x A_matrix_inversion(3, 3). It is a unit matrix
Public Result_I (3,3)
'Matrix used to verify the traspose of A_matrix()
Public Result_A_Transpose(3, 3)
'Matrix used to verify the inversion of A_matrix().
'Result_A_Inversion(3, 3) should be equal to A_matrix_inversion(3, 3)
Public Result_A_Inversion(3,3)

'Define Data Tables
DataTable (Test,1,-1)'Set table size to # of records, or -1 to autoallocate.
DataInterval (0,15,Sec,10)
Minimum (1,Batt_volt,FP2,False,False)
Sample (1,PTemp,FP2)
EndTable

'Main Program
BeginProg
Scan(1,Sec,0,0)
PanelTemp (PTemp,15000)
Battery (Batt_volt)
Matrix(1, A_matrix, A_matrix, Result_A_plus_A)
Matrix(2, A_matrix, A_matrix, Result_0)
Matrix(3, A_matrix, A_matrix_inversion, Result_I)
Matrix(4, A_matrix, 0, Result_A_Transpose)
Matrix(5, A_matrix, 0, Result_A_Inversion)
CallTable Test
NextScan
EndProg
```

## Remarks

The maximum size of the matrix is 4 x 4.

A, B, and Result are two-dimensional arrays (matrices). The Option can be:

| Alphanumeric | Description           |
| ------------ | --------------------- |
| 1            | Result = A + B        |
| 2            | Result = A - B        |
| 3            | Result = A \* B       |
| 4            | Result = Transpose(A) |
| 5            | Result = Inverse(A)   |

The dimensions of A(m,n), B(p,q), Result(r,s) are restricted as follows:

- Transpose and Inverse: m = n = r = s

- Add and Subtract: m = p = r, n = q = s

- Multiply: n = p, r = m, s = q

**NOTE:** Options 4 and 5 (Transpose and Inverse) do not use B.

**NOTE:** If the determinant of A is 0 for Inverse(A), NAN will result.

## Parameters

# Option

Specifies the matrix operation to perform with the two-dimensional arrays, A and B. The Option can be:

| Alphanumeric | Description           |
| ------------ | --------------------- |
| 1            | Result = A + B        |
| 2            | Result = A - B        |
| 3            | Result = A \* B       |
| 4            | Result = Transpose(A) |
| 5            | Result = Inverse(A)   |

# A, B

Two- dimensional array for matrix math.

Type: Variable array

# Result

Two-dimensional array holding the result of matrix math performed with arrays A and B.

Type: Variable array
