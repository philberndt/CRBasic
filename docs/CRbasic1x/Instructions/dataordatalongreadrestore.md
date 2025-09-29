# Data or DataLong, Read, Restore (Define Data, Read Data, Restore Data)

Data (or DataLong) is used to define a list of constants that can then be read (using Read) into a variable array later in the program.

## Syntax

Data (or DataLong)list of constants

Read[VarExpr]

Restore

The program below uses Data to hold the data values and Read to transfer the values to variables.

```
Data1, 2, 3, 4, 5'data for x
Data6, 7, 8, 9, 10'data for y

Public X(5), Y(5)'declare public variables
Dim I'declare private variables

BeginProg
For I = 1 To 5
Readx( I )'Read first five constants into X array
Next I
For I = 1 To 5
Ready( I )'Read first five constants into Y array
Next I
EndProg
```

This next example uses Restore to read 1, 2, 3, 4 into both X( ) and Y( ) variables.

```
Data1, 2, 3, 4'data for x and y

Public X(4), Y(4)'declare public variables
Dim I'declare private variables

BeginProg
For I = 1 To 4
ReadX( I )'Read all 4 constants into X array
Next I
Restore'Reset read pointer back to the first value in the list defined by data
For I = 1 To 4
ReadY( I )'Read all 4 constants into Y array
Next I
EndProg
```

This third example is the same as above except that DataLong is used instead of Data.

```
DataLong18987653, 2147000000, 35800, 4500000'data for x and y

Public X(4)As Long, Y(4) As Long'declare public variables
Dim I'declare private variables

BeginProg
For I = 1 To 4
ReadX( I )'Read all 4 constants into X array
Next I
Restore'Reset read pointer back to the first value in the list defined by data
For I = 1 To 4
ReadY( I )'Read all 4 constants into Y array
Next I
EndProg
```

## Remarks

The Data statement is used to define a list of floating point constants. The DataLong statement is used to define a list of Long constants. Each constant in the list is separated by a comma. Data and DataLong can also use expressions if those expressions evaluate to a constant.

The Read statement is used to begin reading constants from the list defined by Data or DataLong into a variable array. A subsequent Read picks up where the last Read left off. The Read function does not assume a data type; therefore, it is up to the user to ensure that the variable/variable array into which the constants are loaded is the correct type (Float or Long).

The Restore statement is used to reset the location of the Read pointer back to the first value in the list defined by Data or DataLong. The next Read following Restore will begin with the first value of the data list.
