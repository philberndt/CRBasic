# DHCPRenew

The DHCPRenew instruction restarts DHCP on the datalogger's Ethernet interface.

## Syntax

DHCPRenew

In the following example program, once per hour the datalogger toggles a control port (which might trigger power to an IP modem), and then requests an IP address before making a callback attempt to a LoggerNet server with address 4084.

```
Public Batt, PTemp, Temp, IPPort,Result
Dim Scratch

DataTable (TempviaIP,True,1000)
DataInterval (0,1,Min,10)
Minimum (1,Batt,FP2,False,False)
Average (1,Temp,FP2,False)
EndTable

BeginProg
Scan (1,Sec,3,0)
IPPort = TCPOpen ("192.168.7.76",6785,0)
Battery (Batt)
PanelTemp (PTemp,4000)
TCDiff (Temp,1,mv34,1,TypeT,PTemp,True,0,4000,1.0,0)

If IfTime (0,60,Min) Then
PortSet (C1,1)'set port to trigger modem power
DHCPRenew
SendVariables (Result,IPPort,0,4084,0000,0,"Public","Callback",Scratch,1)
PortSet (C1,0)'turn off modem power
EndIf

CallTable (TempviaIP)
NextScan
EndProg
```

## Remarks

When this instruction is executed, the datalogger broadcasts a request for a new DHCP lease from an available DHCP server.

This instruction is used in instances where the datalogger is using a dynamically assigned IP address, but the address on the network changes.
