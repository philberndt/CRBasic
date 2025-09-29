# PanelTemp (Panel Temperature)

The PanelTemp instruction is used to read the temperature of the dataloggerwiring paneland output the value in degrees Celsius.

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
PanelTemp(PTemp,15000)
Calltable (Test)
NextScan
EndProg
```

## Remarks

The measurement from the PanelTemp instruction can be used as the reference temperature for thermocouple measurements.

## Parameters

# Dest (Destination)

The Variable in which to store the results of the instruction. Right-click the parameter to display a list of defined variables.

If this instruction has a Repetitions parameter and it is greater than 1, the results are stored in an array with the variable name. The array must be dimensioned large enough to hold all of the values returned from all of the Reps.

Type: Variable or Array

# fN1

Determines the lowest frequency that will be eliminated or notched out by the sinc filter. This filter notches out frequencies at integer multiples of fN1by averaging for a time equal to 1/fN1; thus, lower fN1frequencies result in longer measurement times.Set fN1to the lowest frequency that should be filtered out. For example, setting fN1to 60 filters all multiples of 60 and above (60, 120, 180, etc.) but will not effectively filter lower frequencies; for example 30 Hz.Any value between 0.5 Hz and 31.25 kHz can be entered, but common options for filtering noise are:

| Option         | Description                                                       |
| -------------- | ----------------------------------------------------------------- |
| 15000          | Performs a 0.0667 millisecond integration (for fast measurements) |
| 60 (or \_60Hz) | Performs a 16.67 millisecond integration (filters 60 Hz noise)    |
| 50 (or \_50Hz) | Performs a 20 millisecond integration (filters 50 Hz noise)       |

Type: Constant
