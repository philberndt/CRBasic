# SortSpaIndexed (Sort Array Elements and Maintain Index Information)

The SortSpaIndexed instruction sorts a one-dimensional array in ascending order while keeping the original index information.

## Syntax

SortSpaIndexed(Dest,Swath,Source)

This program takes the Source arrays, Farray and Iarray, and sorts them into the Dest arrays, fout and iout, respectively.

```
'Declare Variables
Public Farray(6) = {3.5,1.2,4.7,3.6,6.8,9.3}
Public Iarray(6) as Long = {3,1,4,20,15,8}

Public fout(6,2)
Public iout(6,2) as long

'Main Program
BeginProg
Scan(1,sec,0,0)
SortSpaIndexed(fout,6,Farray)
SortSpaIndexed(iout,6,Iarray)
NextScan
EndProg
```

The results of this program are shown here. The first dimension of the Dest arrays (fout and iout) contain the sorted values of the Source arrays (Farray and Iarray) in the first dimension and the original index position of each value in the second dimension.

## Remarks

The SortSpaIndexed instruction sorts in ascending order the one-dimensional array in the Source parameter, containing Swath elements, into a two-dimensional array, Dest. The destination array contains the sorted values in the first dimension of the array and the original index position of those values in the second dimension of the array.

## Parameters

# Dest (Destination)

The variable in which to store the results of the instruction. The destination must be an N x 2 array where N is the number of elements that are being sorted. The Destination array will contain the sorted values in the first dimension of the array and the original index position of those values in the second dimension of the array. Right-click the parameter to display a list of defined variables.

Type: 2-Dimensional Array

# Swath

The number of elements contained in the array to be sorted.

Type: Constant

# Source

The variable array to be sorted.

Type: 1-Dimensional Array
