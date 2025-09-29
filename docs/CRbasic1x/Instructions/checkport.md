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

CheckPort returns True (-1) if the specifieddigital channelis high or False (0) if thedigital channelis low. CheckPort can be used on the right side of an expression (e.g., Variable = CheckPort (Port)) or as an expression. CheckPort has only one parameter:

## Parameter

# Port

The number of the port to use in this instruction. An alphanumeric code is entered. Right-click to display a list.

| Code | Description        |
| ---- | ------------------ |
| C1   | Control terminal 1 |
| C2   | Control terminal 2 |
| C3   | Control terminal 3 |
| C4   | Control terminal 4 |
| C5   | Control terminal 5 |
| C6   | Control terminal 6 |
| C7   | Control terminal 7 |
| C8   | Control terminal 8 |

Type: Constant

**Caution:** The value returned may not be valid if using the control port as a serial port or as a pulse counting port.
