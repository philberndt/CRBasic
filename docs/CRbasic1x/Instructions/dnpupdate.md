# DNPUpdate (Update DNP)

The DNPUpdate instruction sets up the datalogger as a DNPSlave device and determines when it will update its arrays of DNP elements.

## Syntax

DNPUpdate(DNPSlaveAddr,DNPMasterAddr,TimeOut[optional],Retries[optional],ConnectHandle[optional] )

Example 1. This program sets up a DNP3 array of 2 containing Panel Temperature in the first index and Battery Voltage in the second index. Events are triggered if the panel temperature is above 25 C or if the battery voltage drops below 11.5 volts.

```
Public PTemp, batt_volt

Public DNP3_Data(2) As Long'Declare array for DNP3 Data
Public DNP3_Flag(2) As Long'Declare array for DNP3 Data Quality Flags
Public Event_Trigger(2) As Boolean 'Declare event trigger

'Main Program
BeginProg
DNP(20000,115200,1000)
DNPVariable(DNP3_Data(),2,30,1,0,DNP3_Flag(),0,0)'DNPVariable instruction for static data
DNPVariable(DNP3_Data(),2,32,1,1,DNP3_Flag(),Event_Trigger(),10)'DNPVariable instruction for event data
DNP3_Flag(1) = 1'initialize data quality flag for Ptemp to be "online"
DNP3_Flag(2) = 1'initialize data quality flag for Battery voltage to be "online"

Scan (1,Sec,0,0)
PanelTemp (PTemp,15000)
Battery (batt_volt)

If PTemp > 25 Then'Trigger change event if Ptemp > 25
Event_Trigger(1) = True
Else
Event_Trigger(1) = False
EndIf

If batt_volt < 11.5 Then'Trigger change event if batt_volt < 11.5
Event_Trigger(2) = True
Else
Event_Trigger(2) = False
EndIf

DNP3_Data(1) = PTemp * 100'Scaling by 100
DNP3_Data(2) = batt_volt * 100'Scaling by 100

DNPUpdate(1,10)'Slave Address of 1 and Master Address of 10
NextScan
EndProg
'///////////////END OF PROGRAM 1\\\\\\\\\\\\\\\\\\\
```

Example 2. The following program example demonstrates using analog output object 41 to set an analog value and object 40 to report the status of an analog output point. In this case, a counter is used for demonstration.

```
'DNP3 Program for Analog Outputs
'Declare DNPvariable types ot match the DNP variation
'Var 1 = 32 bit
'Var 2 + 16 bit
'Var 3 = floating point

Public Link_Conf As Long
Public Batt_volt
Public Obj40Var As Long'Use type Float for 41 variation 3; se Long for other variations
Alias Obj40Var = Stage_height
Public Obj40Var_Flag as Long
Public Counter'To generate data for demonstration

BeginProg
Link_Conf = 1000'Disables datalink confirmation if using serial comms
Obj40Var_fLAG = 1
DNPVariable(Obj40Var,1,40,1,0,Obj40Var_Flag(),0,0)'Make variations for objects 40 and 41 the same
DNPVariable(Obj40Var,1,41,1,0,Obj40Var_Flag(),0,0)'Object 41 is used for writing value, 40 is used to read it back out
Scan (1,Sec,0,0)
Counter += 1'Increment counter each scan
Stage_height = Counter
'DNP(ComC1,115200,Link_Conf) 'Serial Example
DNP(20000,115200,Link_Conf)'IP Example
Battery (Batt_volt)
DNPUpdate(1,10)'Slave Address of 1 and Master Address of 10
NextScan
EndProg
'////////////////End Of Program 2\\\\\\\\\\\\\\\\\
```

Example 3. Send Unsolicited Response from Datalogger 1 via TCP/IP using optional parameter, ConnectHandle:

```
Public Binput (2) As Boolean
Public counter As Long
```

Public IP_Socket As Long
Const = IPAddress ="192.168.xx.xx"
BeginProg
DNP(20000,115200,1000)
DNPVariable(Binput(),2,1,2,0,0,0,0)
DNPVariable(Binput(),2,2,1,1,0,0,2)
Scan(1,Sec,1,0)
IP_Socket = TCPOpen(IPAddress,20000,0)
counter = counter +1
DNPUpdate(1,10,5,0,IP_Socket)
NextScan
EndProg

```

## Remarks


This instruction must execute in order for the DNP outstation to update its arrays. It is typically placed in a program after the elements in the array are updated. If this instruction is not included in a program, the DNP array will never be updated with new values. The datalogger can support communication for up to three masters. If communications are required for multiple masters, a DNPUpdate instruction must be included in the datalogger program for each one.

When a DNP master comes in with a request, the amount of time to process the scan running DNPUpdate can increase. If DNPUpdate is in the main scan, increasing the number of scan buffers can help to avoid skipped scans.


## Parameters



# DNPSlaveAddr (DNP Outstation Address)


Assigns an address to the DNP outstation. The valid address range is 1 - 65520.

Type: Constant or Variable


# DNPMasterAddr (DNP Master/Client Address)


Assigns the address of the DNP master / client to which the datalogger will respond. Valid address range is 1 - 65520. The datalogger will respond to any DNP master regardless of its address. Up to 3 masters are supported.

Type: Constant or Variable


### Optional Parameters


Unsolicited response transmission can be enabled in the datalogger. This allows the datalogger to transmit changes or events without having received a specific request for the data. This mode is useful if the master station requires notification as soon as possible after a change occurs, rather than waiting for the master station to poll the outstation.

If the unsolicited response parameters are not present or if the unsolicited response confirmation timeout is 0, then unsolicited responses are disabled.


# Timeout (Response Wait Time)


The time, in seconds, that the datalogger will wait for confirmation that an unsolicited response was received, before attempting the transmission again. If this value is set to 0, unsolicited responses are disabled.

Type: Constant


# Retries


The number of times the datalogger will attempt a transmission if the initial attempt fails. If this parameter is 0, then unsolicited responses (with data) will retry forever or until confirmed by the master.

Type: Constant


# ConnectHandle (Connect Handle)


A variable that is controlled by the TCPOpen instruction that connects the master station as if the datalogger received an incoming request. This is useful in the case that the outstation needs to make a TCP connection instead of the master station.

Type: Variable
```
