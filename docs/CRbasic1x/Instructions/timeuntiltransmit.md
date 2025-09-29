# TimeUntilTransmit (Time until Transmit)

The TimeUntilTransmit function returns the time remaining, in seconds, before communication with the host datalogger. A typical use of this instruction is to trigger the execution of the [SendGetVariables](sendgetvariables.md) instruction when the datalogger's communication time slot occurs (for example, If TimeUntilTransmit = 0 Then SendGetVariables).

## Syntax

TimeUntilTransmit

In this example, a datalogger with PakBus address 109 is programmed to send three variables to a datalogger with an address of 1 using the TimeUntilTransmit and SendGetVariables instructions. Three variables are sent to address 1 when the TimeUntilTransmit = 0.

Note that the scan rate in this program must be 1 second. For the program that is run in the host datalogger (address 1), see the [Network](network.md) example.

```
'Declare Public Variables
Public RefTemp, batt_volt, RxData
Public TxData(3)
Public RxResponse
Public counter, time

'Define Data Tables
DataTable (Test,1,-1)
DataInterval (0,15,Sec,10)
Minimum (1,batt_volt,FP2,0,False)
Sample (1,RefTemp,FP2)
EndTable

'Main Program
BeginProg
Scan (1,Sec,0,0)
counter=counter+1
If counter=100 Then
counter=0
EndIf
PanelTemp (RefTemp,15000)
Battery (Batt_volt)
TxData(1)=RefTemp
TxData(2)=Batt_volt
TxData(3)=counter

time =TimeUntilTransmit

If time = 0 Then
SendGetVariables(RxResponse,ComRS232,0,1,0000,0,TxData,3,RxData,1)
EndIf

CallTable Test
NextScan
EndProg
```

## Remarks

The TimeUntilTransmit instruction must be placed in the Main program scan.

The TimeUntilTransmit value is derived from the time slot information that is sent by the host datalogger. If the host datalogger has not yet sent time slot information, this instruction will use a random time interval between 0 and 60 seconds until communication with the host is made.
