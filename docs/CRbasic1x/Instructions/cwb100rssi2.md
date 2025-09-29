# CWB100RSSI (Poll Radio Signal Strength of CWB100)

The CWB100RSSI instruction is used to poll the wireless sensors in a network for radio signal strength.

## Syntax

CWB100RSSI([CWBPort](../parameters/cwbport1.md))

In the example program below, a 109 probe attached to a CWS900 and a CS655 are measured in a wireless network. No configuration string is used in the CWB100 instruction; thus, the two sensors will be added to the CWSArray as they are found, and default variable names will be used.

Additional configuration strings are given as comments in the program, to provide an example of the different ways in which a configuration string can be used to control the names of the variables stored by the datalogger.

Note that ArrayIndex is also used in two of the data tables to return data values of measurements whose indices are unknown at the time of programming.

Every four hours the CWB100Routes and CWB100RSSI instructions are run to return the routes of the sensors in the network and the radio signal strength of each sensor. Prior to the execution of the CWB100RSSI instruction, the values for RSSI in both sensors will be reflected asNAN

**Note:** Not a number. A data word indicating a measurement or processing error. Voltage overrange, SDI-12 sensor error, and undefined mathematical results can produce NAN.

.

```
Const NumValues = 12
Public CWSArray(NumValues), WirelessRoutes As String * 50
Public ConfigString As String * 125 = "12530A40138D CWS109 4 Temp IntTemp Batt Signal,12530A4013A7 CWS655 8 VH20 EC Temp Period AR IntTemp Batt Signal"
Public PTemp, BattV

DataTable (WSN5min,True,1000)
DataInterval (0,5,min,10)
Sample (NumValues,CWSArray(),FP2)
EndTable

DataTable (WSN60min,True,1000)
DataInterval (0,1,Hr,10)
Average (1,PTemp,FP2,False)
Minimum (1,BattV,FP2,False,False)
Average (1,CWSArray(ArrayIndex("CWS900_1_Ts")),FP2,False)
FieldNames ("CWS900_Temp")
Average (3,CWSArray(ArrayIndex("CWS655_1_VWC")),FP2,False)
FieldNames ("CWS655_VWC")
EndTable

DataTable (Diag,True,60)
Sample (1,WirelessRoutes,String)
Sample (1,CWSArray(ArrayIndex("CWS900_1_SS")),FP2)
FieldNames ("CWS900_Signal")
Sample (1,CWSArray(ArrayIndex("CWS655__1_SS")),FP2)
FieldNames ("CWS655_Signal")
EndTable

BeginProg
Scan (5,min,3,0)
PanelTemp (PTemp,250)
Battery (BattV)

'No Config string, network autodiscovery
CWB100(C1,CWSArray())

'>>>>>Other example config strings<<<<<
'Config string using file stored on CP
'CWB100 (C1,CWSArray(),"CPU:CWSConfig.txt")

'Config string for 109 probe and CWS655, all values w/unique names
'CWB100 (C1,CWSArray(),"12530A40138D CWS109 4 Temp IntTemp Batt Signal,12530A4013A7 CWS655 8 VH20 EC Temp Period AR IntTemp Batt Signal")
'Config string using variable for string
'CWB100 (C1,CWSArray(),ConfigString)

'Config string for 109 probe and CWS655, 2 values and 6 values (no BV or SS) w/unique names
'CWB100 (C1,CWSArray(),"12530A40138D CWS109 2 Temp IntTemp,12530A4013A7 CWS655 6 VH20 EC Temp Period AR IntTemp")
'Every four hours, return network routing and get sensor strength
If IfTime (0,4,Hr) = True Then
CWB100Routes(C1,WirelessRoutes)
CWB100RSSI(C1)
CallTable (Diag)
EndIf

CallTable (WSN5min)
CallTable (WSN60min)
NextScan

EndProg
```

## Remarks

When the CWB100RSSI instruction is executed, a separate RF transmission goes out to the wireless sensors in the network. The RSSI values are returned during the next measurement cycle along with the other measurements. RSSI values range from 0 (worst) to 32 (best). The RSSI values for each sensor in the network will beNAN

**Note:** Not a number. A data word indicating a measurement or processing error. Voltage overrange, SDI-12 sensor error, and undefined mathematical results can produce NAN.

if the CWB100RSSI instruction has not been executed. If a sensor communicates with the base through other sensors (hops), the signal strength returned is from the sensor to its closest hop.

This instruction should not be executed frequently so that power drain and network traffic is not unnecessarily used.

## Parameter

# CWBPort (CWB Terminal)

The CWBPort parameter is used to specify the terminal pair on the datalogger to which the wireless sensor base s Data/A terminal will be connected. Valid options are C1, C3, U1, U3, U5, U7, U9, or U11.

Right-click the parameter to display a drop-down list of valid options.

Type: Constant
