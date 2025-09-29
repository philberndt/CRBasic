# Mathematical Expressions

Mathematical expressions can be entered algebraically into program code to perform processing on measurements, to be used for logical evaluation, or to be used in place of some parameters.

## Measurement Processing

To convert a thermocouple measurement from degrees Celsius to degrees Fahrenheit, you could use the following expression:

```
TCTempF=TCTemp(1)*1.8+32
```

## Logical Evaluation

Expressions could be used to determine the flow of a program:

```
IfTCTemp(1) > 100Then
CallSubroutine1
Else
'enter code for main program
EndIf
```

## For Parameters

Many parameters will allow the entry of expressions. In the following example, the DataTable is triggered, and therefore data stored, if TCTemp(1)>100.

```
DataTable(TempTable, TCTemp(1)>100, 5000)
```
