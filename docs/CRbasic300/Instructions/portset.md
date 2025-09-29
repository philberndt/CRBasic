# PortSet (Set Port)

The PortSet instruction is used to set a port high or low.

## Syntax

```
PortSet
```

(Port,State)

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
PanelTemp (RefTemp,4000)
TCDiff (Tblk1(),1,mv34,1,TypeT,RefTemp,True,0,4000,1.8,32)

CallTable TEST
PortSet(C1, 0)'Set port 1 low
NextScan
EndProg
```

## Remarks

This instruction is controlled by the task sequencer, which sets up the measurement order. This results in the PortSet instruction always occurring directly after the measurement instruction preceding it in the program, regardless of whether or not the PortSet instruction is in a conditional block. To write to a port conditionally, use the [WriteIO](writeio.md) instruction.

The [Delay()](delay3.md) instruction can be used to set a delay between two PortSet instructions. When the PortSet() is running in a slow sequence, the Delay() Option must be **0** and the delay can be no larger than the scan interval of the main scan.

## Parameters

# Port

The number of the port to use in this instruction. Right-click to display a list.

| Code  | Description            | High (V) | Low (V) |
| ----- | ---------------------- | -------- | ------- |
| C1    | Control Port 1         | ~5       | 0       |
| C2    | Control Port 2         | ~5       | 0       |
| SE1   | Single ended channel 1 | ~3.3     | 0       |
| SE2   | Single ended channel 2 | ~3.3     | 0       |
| SE3   | Single ended channel 3 | ~3.3     | 0       |
| SE4   | Single ended channel 4 | ~3.3     | 0       |
| SW12V | Switched 12V (SW12)    | ~12\*    | 0       |
| P_SW  | Pulse Channel          | ~3.3     | 0       |
| VX1   | Excitation channel 1   | 5        | 0       |
| VX2   | Excitation channel 2   | 5        | 0       |

\* Voltage on a SW12 terminal will change with datalogger supply voltage.

Type: Constant

# State

Determines whether to set the port high or low. Right-click to display a list.

| Value | State |
| ----- | ----- |
| 0     | Low   |
| ≠0    | High  |

Type: Constant, Variable, or Expression
