# MonitorComms (Monitor Communications)

The MonitorComms instruction is used to send communication debugging information to a string variable.

## Syntax

MonitorComms(Dest,ComNum,Output)

The test program below shows how the MonitorComms instruction can be used immediately following a string that is sent out a communication port using SerialOut to monitor that output string. To use the test program, in software set the TriggerOut variable to true and then notice the resulting string in the Monitor variable.

This program was written for aCR1000X. Some parameters may need to be changed (voltage ranges, for example) to compile in other dataloggers.

```
'Declare Variables
Public PTemp, TCTemp, Monitor As String * 500, OutString As String * 100, TriggerOut As Boolean

'Main Program
BeginProg
'Set up the serial port for communication
SerialOpen (COMC1,115200,0,0,1000)

Scan (1,Sec,3,0)
PanelTemp (PTemp,15000)
TCDiff (TCTemp,1,mv200C,1,TypeT,PTemp,True,0,15000,1.0,0)

'Create a string that will be transmitted
OutString = "The temperature is " + FormatFloat(TCTemp,"%3.2f" ) + " degrees C"
'Set TriggerOut to execute the following code, which send OutString viaCOMC1
If TriggerOut Then
SerialOut (COMC1,OutString,"",0,0)
MonitorComms(Monitor,COMC1,1)
TriggerOut = False
EndIf
NextScan
EndProg
```

## Remarks

The datalogger has a "comms watch" terminal communication mode (mode W) that lets you watch communication traffic on a specified port. This is useful for debugging communication with other devices, such as an SDI12 or serial sensor. The W mode in the datalogger streams ASCII or binary information to a terminal window. The MonitorComms instruction writes this same information to the string variable defined by the Dest parameter.

## Parameters

# Destination

A string variable in which to hold the communication information coming from the specified port.

Type: String Variable

# ComNum

Specifies the port on which to monitor communication traffic. Right-click the parameter within the CRBasic Editor to see a list of valid port options.

Type: Constant

# Output

Determines whether the resulting string is returned in ASCII or Binary format. Enter 1 for ASCII or 0 for Binary.

Type: Constant
