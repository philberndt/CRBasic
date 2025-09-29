# PortGet (Status of Port)

The PortGet instruction is used to read the status of one of the control ports.

## Syntax

PortGet(Dest,Port)

This example uses PortGet to read the status of port 1 at the beginning of each scan.

```
'variables
Public TCTemp,PortStatus
Public RefTemp

'Table declaration
DataTable( TEST,1, 1000 )'Trigger, buffer of 1000 Records
DataInterval( 0, 1,Sec,10 )
Average( 1, TCTemp,FP2,False )
Sample ( 1, PortStatus ,FP2 )
EndTable

BeginProg
Scan( 1,2 ,0,0 )
PortGet( PortStatus, C1)'Read port 1
PanelTemp (RefTemp,15000)
TCDiff (TCTemp,1,mv200C,1,TypeT,RefTemp,True,0,15000,1.0,0)

CallTable TEST
NextScan
EndProg
```

## Remarks

This instruction will read the status of the specified port and place the result in the Dest variable. This instruction is controlled by the task sequencer, which sets up the measurement order. This results in the PortGet instruction always occurring directly after the measurement instruction preceding it in the program, regardless of whether the PortGet instruction is in a conditional block. If it is desired to read the status of a port conditionally, the [ReadIO](readio.md) instruction should be used.

## Parameters

# Dest (Destination)

The Variable in which to store the results of the instruction. Right-click the parameter to display a list of defined variables.

If this instruction has a Repetitions parameter and it is greater than 1, the results are stored in an array with the variable name. The array must be dimensioned large enough to hold all of the values returned from all of the Reps.

Type: Variable or Array

For the PortGet instruction, Dest variable, a 1 is stored if the port is high; 0 is stored if the port is low.

# Port

The terminal to use in the instruction. An alphanumeric code is entered. Right-click to display a list.

| Code   | Description    |
| ------ | -------------- |
| C1     | Control Port 1 |
| C2     | Control Port 2 |
| C3     | Control Port 3 |
| C4     | Control Port 4 |
| C5     | Control Port 5 |
| C6     | Control Port 6 |
| C7     | Control Port 7 |
| C8     | Control Port 8 |
| SW12_1 | SW12-1         |
| SW12_2 | SW12-2         |

Type: Constant
