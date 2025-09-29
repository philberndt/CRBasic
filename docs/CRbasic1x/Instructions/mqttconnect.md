# MQTTConnect

Attempts to connect or disconnect MQTT over an available IP connection.

## Syntax

MQTTConnect(DesiredState)

**NOTE:** MQTTConnect() is not required for a persistent MQTT application. MQTT will attempt to connect when enabled with the Device Configuration setting. However, MQTTConnect() can be used to disconnect at a certain time, or after a certain event and then reconnect. The following cell modem application is a common example of when MQTTConnect() is useful.

The following example shows the use of the MQTTConnect() when using the datalogger to control power to a cellular modem using the switched 12V (SW12V) terminal on the datalogger. This is useful in applications where the modem uses more power than the datalogger.

In this example, the TimeIsBetween instruction is used to turn on SW12 for 15 minutes every 60 minutes between 9:00 a.m. and 5:00 p.m.

```
'In this example, MQTTConnect is used to open an MQTT session when a modem is turned on.

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

P3Open = PPPOpen
```

MQTTConnect(True)
Else
P3Close = PPPClose
MQTTConnect(False)

SW12State = False
EndIf

'Always turn OFF SW12 if battery drops below 11.5 volts
If BattV<11.5 ThenSW12State = False
'Set SW12-1 to the state of 'SW12State' variable
SW12(SW12_1,SW12State)

'Call Data Tables and Store Data
CallTable Table2
NextScan
EndProg

```

## Remarks


**NOTE:** This instruction is not required for a persistent MQTT application. MQTT will attempt to connect when enabled with the Device Configuration setting.


## Parameter



# DesiredState


| Logic | Description |
| --- | --- |
| True or 1 | Attempts to connect MQTT over an available IP connection. |
| False or 0 | Loads the job queue with a disconnect request. The disconnect request will follow any active jobs in the queue. This allows the datalogger to complete the publishing of data before closing the connection. Since the disconnect request is in a buffered queue, the disconnect will not always happen immediately. |

Type: Constant or Variable
```
