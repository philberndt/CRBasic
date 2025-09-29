# GOESField (Send Fields to GOES)

The GOESField instruction is used along with the [GOESTable](goestable.md) instruction to specify output fields in a data table to be sent to a GOES satellite transmitter (that is, TX325, TX321, TX320, or TX312). When a table containing GOESTable and GOESField stores a new record, then the datalogger will send the specified data to the satellite transceiver.

## Syntax

GOESField(NumVals,Decimation,Precision,Width,SHEF)

The following code snippet demonstrates the use of GOESField() in a data table. Note that a compile error will occur if GOESField() is declared without first declaring GOESTable(). The GOESField instruction must precede each field in the data table that is to be output to the transmitter. A full example program can be found in the [GOESTable](goestable.md) instruction example.

```
DataTable (TimedTable,1,9999
DataInterval (0,30,Sec,10)
GOESTable(Result,ComPort,model,0,FieldsScanOrder,NewestFirst,format)
Average (2,Vals,FP2,FALSE)
FieldNames("Average1,Average2")

GOESField(numvals,decimate,precision,width,Shef)
Sample (1,val1,ieee4)

GOESField(numvals(2),decimate(2),precision(2),width(2),Shef(2))
Sample (1,val2,ieee4)
EndTable
```

## Remarks

The GOESField instruction must precede each field in the data table that is to be output to the transmitter. If there is no GOESField() declaration prefacing an output instruction, then there will be no output of data from that field to the transmitter. A compile error will occur if a GOESField instruction is declared in a data table that does not use the GOESTable instruction.

## Parameters

# NumVals (Number of Historical Values)

The number of historical values (time series) of the field to output. For example, if the values of the field through time were (oldest to newest):

10.1,10.2,10.3,10.4,10.5,10.6,10.7

Then NumVals = 4 with Decimation =1 would give:

10.4, 10.5,10.6,10.7

**NOTE:** The Newest_First parameter of [GOESTable](goestable.md) determines whether newest or oldest data are output first.

Beginning with OS4, if NumVals is 0, new output is appended to the output table in the order set by the Fields_Scan_Order parameter of the [GOESTable](goestable.md) instruction. For example:

| NumVals | Fields_Scan_Order | Output Appended To: |
| ------- | ----------------- | ------------------- |
| 0       | False             | End of last line    |
| 0       | True              | Separate line       |

Type: Constant or Variable

# Decimation

1 = output every value, 2 = output every other value, 3, output every third value

Type: Constant or Variable

# Precision

For GOESTable formats 2 and 3, precision is the number of digits to the right of the decimal. For GOESTable formats 4 and 5, precision is the power of 10 that the value will by multiplied by before taking its integer value. Precision is ignored for GOESTable formats 0 and 1 (Campbell ScientificFloating Point ASCII).

Type: Constant or Variable

# Width (Number of Characters in Field)

Width specifies the number of characters in the field. The maximum number of characters is 13. If the actual number of characters in the field is larger than specified by the Width parameter, then precision will be adjusted downward in an attempt to fit the number of characters. The Width parmeter is ignored for GoesTable format 1 (Campbell ScientificFloating Point ASCII).

Type: Constant or Variable

# SHEF (SHEF PE Code)

The SHEF parameter is a string variable used to specify a SHEF PE code. Empty quotes (for example "") means no SHEF code is specified.

Type: Variable as string
