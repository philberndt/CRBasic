# PPPClose (Close PPP Connection)

The PPPClose function closes an opened PPP connection with a server.

## Syntax

_variable_ = PPPClose

The following example shows the use of the PPPOpen and PPPClose functions when using the datalogger to control power to a cellular modem using the switched 12V (SW12V) terminal on the datalogger. This is useful in applications where the modem uses more power than the datalogger.

In this example, the TimeIsBetween instruction is used to turn on SW12 for 15 minutes every 60 minutes between 9:00 a.m. and 5:00 p.m.

```
'Declare Variables and Units
Public BattV
Public SW12State As Boolean
Public P3Open as String, P3Close

Units BattV=Volts

'Define Data Tables
DataTable(Table2,True,-1)
DataInterval(0,1440,Min,10)
Minimum(1,BattV,FP2,False,False)
EndTable

'Main Program
BeginProg
'Main Scan
Scan(5,Sec,1,0)
'Set SW12-1 to the state of SW12State variable
SW12(Sw12_1,SW12State)

'Datalogger Battery Voltage measurement 'BattV'
Battery(BattV)

'SW12 Timed Control
'Turn ON SW12 between 0900 hours and 1700 hours
'for 15 minutes every 60 minutes
If TimeIsBetween(540,1020,1440,Min) And TimeIsBetween(0,15,60,Min) Then
SW12State=True

P3Open =PPPOpen
Else
P3Close =PPPClose

SW12State=False
EndIf

'Always turn OFF SW12 if battery drops below 11.5 volts
If BattV<11.5 ThenSW12State=False
'Set SW12-1 to the state of 'SW12State' variable
SW12(SW12_1,SW12State)

'Call Data Tables and Store Data
CallTable Table2
NextScan
EndProg
```

## Remarks

The PPPClose function has no parameters. This function returns -1 (true) if the session was successfully closed or 0 (false) if the function fails. The datalogger may wait up to 10 seconds before moving to the next instruction if the server is not responding to PPPClose.
