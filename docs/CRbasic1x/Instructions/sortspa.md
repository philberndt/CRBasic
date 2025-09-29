# SortSpa (Sort Array Elements)

The SortSpa function is used to sort the elements of an array in ascending order.

## Syntax

SortSpa(Dest,Swath,Source, [Dimension] (optional) )

In the following example program, three variables from an array are sorted and stored into a new variable array.

```
Public Temp(3), RefTemp, SortDest(3)
BeginProg
Scan (1,Sec,3,0)
PanelTemp (RefTemp,15000)
TCDiff (Temp,3,mv200C,1,TypeT,RefTemp,True,0,15000,1.0,0)

SortSpa(SortDest,3,Temp())
NextScan
EndProg
```

To sort columns by rows, the program can transpose the arrays before using SortSpa and then transpose the result back, as in the example below.

```
Public Out (3,5)
Public In(3,5) = {3,2,1,5,4,22,11,55,44,33,333,222,111,555,444}

'have
'3, 2, 1, 5, 4
'22, 11, 55, 44, 33
'333, 222, 111, 555, 444

'desired sort
'2, 3, 4, 5, 1
'11, 22, 33, 44, 55
'222, 333, 444, 555, 111

BeginProg

Dim i As Long, j As Long
Dim Tin(5,3), Tout(5,3)

For i = 1 to 3
For j = 1 to 5
Tin(j,i) = In(i,j)
Next j
Next i
SortSpa (Tout,5,Tin(1,2),2)
For i = 1 to 5
For j = 1 to 3
Out(j,i) = Tout (i,j)
Next j
Next i

EndProg
```

## Remarks

The results from SortSpa can be stored in the same variable or a different variable. If the results are stored in a different variable, the array is copied from Source and stored into Dest prior to sorting. If the Source and Dest variables are the same, then the sorting is done in place.NAN

**Note:** Not a number. A data word indicating a measurement or processing error. Voltage overrange, SDI-12 sensor error, and undefined mathematical results can produce NAN.

s andINF

**Note:** A data word indicating the result of a function is infinite or undefined.

s are sorted to the top of the array (that is, the most minimum value).

The variables for Dest and Source must be declared as the same type (FP2

**Note:** Two-byte floating-point data type. Default datalogger data type for stored data. While IEEE four-byte floating point is used for variables and internal calculations, FP2 is adequate for most stored data. FP2 provides three or four significant digits of resolution, and requires half the memory as IEEE4.

,IEEE

**Note:** Four-byte, floating-point data type. IEEE Standard 754. Same format as Float.

, orLong

**Note:** Data type used when declaring a variable as an integer.

).

## Parameters

# Dest (Destination)

A variable in which to store the results of the function. Dest must be dimensioned to the size of the swath.

Type: Variable

# Swath

The number of elements in the array to include in the values to be sorted.

Type: Constant

# Source

The first variable in the array for which the sort should be performed.

When using the optional dimension parameter with a two- or three-dimensional array, Source also determines the row, column, or plane to sort on. For example, in a two-dimensional array Source(1,2) will sort on the second column of the array, keeping the rows intact. For example:

```
Public Result (3,2), Source(3,2)
```

SortSpa (Result(1,1),3,Source(1,2),2)'sort 3 rows of 2-D array by 2nd column

```
For a three-dimensional array, the example below demonstrates sorting the planes of the array and then using a For/Next loop to sort the rows of each plane:

```

Public Result(5,4,3), Source(5,4,3)

```
SortSpa(Result(1,1,1),5,Source(1,1,1),3)'sort the planes
```

Dim I

```
For i = 1 To 5
```

SortSpa (Result6(i,1,1),4,Result5(i,1,1),2) 'sort the rows of each plane

```
Next i
```

Type: Variable

## Optional Parameter

# Dimension

An optional parameter that is used to specify the dimension to perform the sort on, in a multi-dimensional array. Valid options are:

- **1 **: Sort on the first dimension of the array (for example, sort on X in Array (X,Y)). This is the default behavior if Dimension is not specified.

- ** 2 **: Sort on the second dimension of the array (for example, sort on Y in Array(X,Y)).

- ** 3**: Sort on the third dimension of the array (for example, sort on Z in Array(X,Y,Z)).

Type: Constant
