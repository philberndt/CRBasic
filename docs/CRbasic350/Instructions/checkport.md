# CheckPort (Check Status of a Control Port)

CheckPort returns the status of acontrolterminal.

## Syntax

CheckPort(Port)

In the following example code, the status of control port C1 is checked and the result is stored in the variable PortStat. The port is set by the state of Flag (if Flag is high, the port is set high).

```
Public PortStat As Boolean
Public Flag As Boolean

BeginProg
Scan (1,Sec,3,0)
If Flag
PortSet (C1 ,1)
Else
PortSet (C1 ,0)
EndIf
PortStat=CheckPort(C1)
NextScan
EndProg
```

## Remarks

CheckPort returns True (-1) if the specifiedportis high or False (0) if theportis low. CheckPort can be used on the right side of an expression (e.g., Variable = CheckPort (Port)) or as an expression. CheckPort has only one parameter:

## Parameter

# Port

The number of the port to use in this instruction. An alphanumeric code is entered. Right-click to display a list.

| Code   | Description                  |
| ------ | ---------------------------- |
| C1     | Control Port 1               |
| C2     | Control Port 2               |
| SE1    | Single ended channel 1       |
| SE2    | Single ended channel 2       |
| SE3    | Single ended channel 3       |
| SE4    | Single ended channel 4       |
| P_SW   | Pulse terminal (P_SW)        |
| P_LL   | Low level terminal (P_LL)    |
| SW12_1 | Switched 12V channel 1       |
| SW12_2 | Switched 12V channel 2       |
| P_VX_1 | Voltage excitation channel 1 |
| P_VX_2 | Voltage excitation channel 2 |

Type: Constant

**Caution:** The value returned may not be valid if using the control port as a serial port or as a pulse counting port.
