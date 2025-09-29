# PortSet (Set Port)

The PortSet instruction is used to set a port high or low.

## Syntax

```
PortSet
```

(Port,State,Option[optional])

This example uses PortSet to set port 1 high at the beginning of each scan, and to set it back low at the end of each scan.

```
Public RefTemp, Tblk1

DataTable( TEST, 1, 1000)
DataInterval( 0, 1,Min, 10)
Average( 1, Tblk1, FP2, 0)
EndTable

BeginProg
Scan( 1,Sec, 1, 0)
PortSet(C1, 1)'Set port 1 high
PanelTemp (RefTemp,15000)
TCDiff (Tblk1(),1,mv200C,1,TypeT,RefTemp,True,0,15000,1.8,32)

CallTable TEST
PortSet(C1, 0)'Set port 1 low
NextScan
EndProg
```

## Remarks

By default, 5 V is applied to the port when the State is set high. Use the PortPairConfig instruction to set the output to 3.3 V.This instruction is controlled by the task sequencer, which sets up the measurement order. This results in the PortSet instruction always occurring directly after the measurement instruction preceding it in the program, regardless of whether or not the PortSet instruction is in a conditional block. To write to a port conditionally, use the [WriteIO](writeio.md) instruction.

The [Delay()](delay3.md) instruction can be used to set a delay between two PortSet instructions. When the PortSet() is running in a slow sequence, the Delay() Option must be **0** and the delay can be no larger than the scan interval of the main scan.

## Parameters

# Port

The number of the port to use in this instruction. Right-click to display a list.

| Code | Description    |
| ---- | -------------- |
| C1   | Control Port 1 |
| C2   | Control Port 2 |
| C3   | Control Port 3 |
| C4   | Control Port 4 |
| C5   | Control Port 5 |
| C6   | Control Port 6 |
| C7   | Control Port 7 |
| C8   | Control Port 8 |

Type: Constant

# State

Determines whether to set the port high or low. Right-click to display a list.

| Value | State |
| ----- | ----- |
| 0     | Low   |
| ≠0    | High  |

Type: Constant, Variable, or Expression

## Optional Parameter

# Option

Optional parameter that determines whether the instruction will run in the measurement task sequence or the processing task sequence; also affects whether the program will compile and run in [SequentialMode or PipelineMode](sequentialmodepipeli2.md):

| Option  | Description                                                                                                |
| ------- | ---------------------------------------------------------------------------------------------------------- |
| Omitted | Instruction is run within the measurement task sequence; program will compile in SequentialMode            |
| 0       | Instruction is run within the measurement task sequence; program will attempt to compile in PipelineMode\* |
| 1       | Instruction is run within the processing task sequence; program will attempt to compile in PipelineMode\*  |

\* other programming may force the program into SequentialMode
