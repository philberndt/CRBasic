# PeakValley (Detect Signal Peaks and Valleys)

The PeakValley instruction is used to detect peaks and valleys (local maxima and minima) in a signal.

## Syntax

PeakValley(DestPV,DestChange,Reps,Source,Hysteresis)

The following code shows the use of the PeakValley instruction in a program.

```
'Declare Variables
Public PeakV(2), Change(3),Deg
Public XY(2)
Const Pi=4*ATN(1)'Define Pi for converting degrees to radians

DataTable(PV1,Change(1),500)'Peaks and valleys for first signal, triggered when Change(1) is not 0.
Sample (1,PeakV(1),IEEE4)'DataTable PV1 holds the peaks and valleys for XY(1)
EndTable

DataTable (PV2,Change(2),500)'Peaks and valleys for second signal, triggered when Change(2) is not 0.
Sample(1,PeakV(2),IEEE4)'DataTable PV2 holds the peaks and valleys for XY(2)
EndTable

'The following table is an alternative to using separate tables for each signal.
'It stores both signals whenever there is a new peak or valley in either signal.
'The value stored for the signal that does not have a new peak will be a repeat
'of its last peak or valley.

DataTable (PVBoth,Change(3),500)
Sample (2,PeakV(1),IEEE4)
EndTable

BeginProg
Scan (500,mSec,0,0)
Deg=Deg+5
XY(1)=Cos(Deg*Pi/180)'Compute the cosine as input XY(1)
XY(2)=Sin(Deg*Pi/180)'Compute the sine as input XY(2)
'Find the peaks and valleys for both inputs, Hysteresis = 0.1
PeakValley(PeakV(1),Change(1),2,XY(1),0.1)
Calltable PV1'Run Table PV1
CallTable PV2'Run Table PV2
CallTable PVBoth'Run Table PVBoth
NextScan'Loop up for the next scan
EndProg'Program Ends Here
```

## Remarks

When a new peak or valley is detected, the new value and the change from the previous value are stored in variables. The minimum amount that the value of the Source array or variable must change for a new maximum or minimum to be detected is set by the Hysteresis argument.

## Parameters

# DestPV

Variable or array in which to store the new peak(s) or valley(s). When a new peak or valley is detected, the value of the peak or valley is loaded into the Dest argument. PeakValley will continue to load the last peak or valley value into the argument until the next peak or valley is detected. If reps is greater than 1, destination must be an array dimensioned to at least the number of reps.

Type -- Variable or Array

# DestChange

Variable or array in which to store the change from the previous peak or valley. When a new peak or valley is detected, the change from the previous peak or valley is loaded in the DestChange argument. When a new peak or valley has not yet been reached, 0 will be stored. When Reps are greater than 1, DestChange must be an array that is dimensioned to Reps+1. The last array element [e.g., DestChange(Reps+1)] is used to flag when a new peak or valley is detected in any of the source inputs. The flag element is stored after the changes and is set to -1 (true) when a new peak or valley is detected and set to 0 (false) when none are detected.

Type -- Variable or Array

# Reps (Repetitions)

The number of repetitions for the measurement or instruction.

Type: Constant integer (or expression that evaluates as a constant).

# Source

The name of the Variable that is the input for the instruction. Right-click the parameter to display a list of defined variables.

Type: Variable

# Hysteresis

The minimum amount that the input source value has to differ from the last peak or valley to be considered a new peak or valley. This would usually be entered as a constant, but the rate of change can also be derived from a variable or expression.

Type Constant, Variable, or Expression
