# PanelTemp (Panel Temperature)

The PanelTemp instruction is used to read the temperature of the dataloggermain processing boardand output the value in degrees Celsius.

## Syntax

```
PanelTemp
```

(Dest,fN1)

The following program shows the use of the PanelTemp instruction.

```
Public PTemp

DataTable (Test,True,-1)
DataInterval (0,1,Min,10)
Sample (1,PTemp,FP2)
EndTable

BeginProg
Scan (1,Sec,3,0)
PanelTemp(PTemp,4000)
Calltable (Test)
NextScan
EndProg
```

## Remarks

The measurement from the PanelTemp instruction does not accurately reflect the temperature of the wiring panel, since it measures the temperature of the main processing board. If the datalogger processor or charge (CHG) inputs are active, the PanelTemp measurement will be warmer than ambient. This should be taken into consideration if this measurement is used as a reference temperature for other measurements such as thermocouples.

## Parameters

# Dest (Destination)

The Variable in which to store the results of the instruction. Right-click the parameter to display a list of defined variables.

If this instruction has a Repetitions parameter and it is greater than 1, the results are stored in an array with the variable name. The array must be dimensioned large enough to hold all of the values returned from all of the Reps.

Type: Variable or Array

# fN1

Determines the lowest frequency that will be eliminated or notched out by the sinc filter. This filter notches out frequencies at integer multiples of fN1by averaging for a time equal to 1/fN1; thus, lower fN1frequencies result in longer measurement times.

| Option | Description                                                                       |
| ------ | --------------------------------------------------------------------------------- |
| 50     | Standard measurement speed; requires 50 msec to filter 50 Hz noise                |
| 60     | Standard measurement speed; requires 50 msec to filter 60 Hz noise                |
| 400    | Medium measurement speed; performs a 6.25 msec integration to filter 400 Hz noise |
| 4000   | Fast measurement speed; performs a 0.5 msec integration to filter 4000 Hz noise   |

Type: Constant
