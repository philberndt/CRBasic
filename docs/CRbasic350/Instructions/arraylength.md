# ArrayLength (Array Length)

The ArrayLength function returns an integer that represents the total number of elements in all the dimensions of an array.

## Syntax

Variable = ArrayLength(ArrayLenVar)

In the following test program, the size of an array can be changed by editing values in the datalogger's constant table. ArrayLength is used to return the size of the array.

```
ConstTable (ConstTableName,0)
Const x=3
Const y=1
EndConstTable

Public Array(X,Y), ArrayLen

BeginProg
Scan (1,Sec,3,0)
ArrayLen =ArrayLength(Array)
NextScan
EndProg
```

## Remarks

The ArrayLenVar parameter is the variable for which to return the number of elements in the array.

In CRBasic, ArrayLength always refers to the number of elements in an array, regardless of the variable type (Float, Long, String, etc.). Thus, ArrayLength returns the number of strings in an array, not the number of characters per string.

ArrayLength can be used to return the number of tables in the datalogger; for example,

NumberofTables(1)=ArrayLength(DataTableInfo.DataTableName)

or

NumberofTables(2)=ArrayLength(Status.DataTableName)

## Parameter

# ArrayLenVar (Array Length Variable)

The variable for which to return the length.

Type: Variable
