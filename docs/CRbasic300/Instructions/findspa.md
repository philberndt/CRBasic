# FindSpa (Find Location of a Value)

The FindSpa function is used to search a source array for a value and return the value's position in the array.

## Syntax

FindSpa(SoughtLow,SoughtHigh,Step,Source)

Use the following test program to see what value is returned in FindResult when the value of each dimension of the FindSource array is set to a number between 1 to 3. The Data instruction is used to load the three-dimensional array with 27 values ranging from 1 to 400.

The program is set up so that the Source Array, as well as the Step and SoughtLow/SoughtHight values can be changed using the Numeric Monitor so that you can see the affect of each change.

```
Public SoughtLow, SoughtHigh, FindStep, FindSource(3,3,3), FindResult
Public x, y, z
Dim i, j, k

Data 1, 3, 50, 25, 99, 305, 400, 2, 8, 45, 30, 88, 100, 6, 73, 2, 4, 51, 26, 98, 304, 399, 201, 9, 46, 31, 89

BeginProg
'Set initial values
SoughtLow=1
SoughtHigh=400
FindStep=1
x=1
y=1
z=1
'Load array with data
For i = 1 To 3
For j= 1 To 3
For k = 1 To 3
Read FindSource(i,j,k)
Next k
Next j
Next i

Scan (1,Sec,3,0)
FindResult=FindSpa(SoughtLow,SoughtHigh,FindStep,FindSource(x,y,z))
NextScan
EndProg
```

## Remarks

The FindSpa function returns the first position in the array of the sought after value. If the value is not found, 0 is returned. SoughtLow and SoughtHigh allow for a range to be specified for the searched value (they are included in the search).

Note that the data types for Source and Sought values must be the same.

## Parameters

# SoughtLow (Lower Limit of Value being Sought)

The lower limit for the value being sought. This value is included in the search.

Type: Variable

# SoughtHigh (Upper Limit of Value being Sought)

The upper limit for the value being sought. This value is included in the search.

Type: Variable

# Step

The step to be used when searching through the source. It is useful in specifying row or dimension when searching a two or three dimensional array.

Note that when Step is used, the value returned takes these "steps" into account. For instance, if a Step of 2 is used in a one dimensional array, a value that is found in the eighth element of an array will return a value of 4. If a Step of 1 was used, the returned value would be 8. Step values greater than one are more often used in multi-dimensional arrays

Type: Variable

# Source

The array to be searched for the sought value. Source can be a one, two, or three dimensional array. In a two dimensional array, the first element is the row and the second is the column. In a three dimensional array, the first element is depth, the second is row, and the third is column.

Type: Variable Array
