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
