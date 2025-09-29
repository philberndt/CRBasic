# IPNetPower (Ethernet Power)

IPNetPower controls power to a specific IP network interface.To control power to all IP network devices simultaneously, use the [EthernetPower](ethernetpower.md) instruction instead.

## Syntax

IPNetPower(Interface,State,Timeout)

In the following program, the IPNetPower instruction is used to turn on/off power to theEthernet portbased on a trigger that allows an alert to be sent by e-mail.

```
'Main program variables
Public Batt, RefTemp, Temp

'declare Email parameter strings (as constants), Message String & Result Variable
Const ServerAddr="mail.mycompany.com"
Const ToAddr="test@yourcompany.com"
Const FromAddr="CR1000X@mycompany.com"
Const Subject="CR1000Xdatalogger message"
Const Attach=""
Const UserName=""
Const Password=""
Const CRLF = CHR(13)+CHR(10)

Public Result As String * 50
Public AlarmTrigger As Boolean
Public Message As String * 250
Public EmailSuccess As Boolean
Public Estate As Boolean

BeginProg
IPNetPower(1,0)'Turn offEthernet portpower
Scan (1,Sec,3,0)
Battery (Batt)
PanelTemp (RefTemp,15000)
TCDiff (Temp,1,mv200C,1,TypeT,RefTemp,True,0,15000,1.0,0)

IPNetPower(1,Estate)'control power with Estate variable
NextScan

SlowSequence
Scan(1,sec,1,0)
If AlarmTrigger = False Then
If Temp > 28 Then AlarmTrigger = True
If AlarmTrigger Then
Estate=True
Message = "Warning!" + CRLF + CRLF
Message = Message + "This is a automatic email message from the datalogger station " + Status.StationName + ". "
Message = Message + "An alarm condition has been identified. "
Message = Message + "The temperature is " + Temp + " degrees C." + CRLF + CRLF + CRlF
Message = Message + "Datalogger time is " + Status.Timestamp
EmailSuccess=EmailSend (ServerAddr,ToAddr,FromAddr,Subject,Message,Attach,UserName,Password,Result)
EndIf
EndIf
If Temp < 28 Then
AlarmTrigger=False
Estate=False
EndIf
NextScan
EndProg
```

## Remarks

The first parameter is used to select the interface to control. The State parameter determines whether the power is on or off. When State is 0, a command is sent to the interface to put it into sleep mode. This instruction is the same as the Ethernet Interface Enabled setting in the datalogger s Settings table. The Timeout parameter is optional and if the state is non-zero, the interface will be powered on and remain on for the time specified by the Timeout. The Timeout is refreshed when activity is detected, thus keeping the interface on while communications are active. The interface will be powered off the number of seconds specified by Timeout after the last detected activity.

To control power to all Ethernet interfaces with one instruction, use the [EthernetPower](ethernetpower.md) instruction.

## Parameters

# IPNetInterface (Interface to Power On/Off)

Used to select the interface to be powered on or off:1 = internal Ethernet;2 = CS I/O Interface(1) (NL201 by default); 3 = CS I/O Interface(2) (NL241 by default);5 = CS Cell2XX-Series cellular modem. Right-click the parameter to display a drop-down list with the valid options.

Type: Constant

# State (Power State)

Determines whether the power is on or off. A non-zero value turns the power on; zero turns the power off. Right click the parameter for a drop-down list.

Type: Constant or variable

# Timeout

The Timeout parameter is the number of seconds the interface will stay powered on after the last activity is detected on the Interface. The Timeout parameter is optional and if the state is non-zero, the interface will be powered on and remain on for the time specified by the Timeout. The Timeout is refreshed when activity is detected, thus keeping the interface on while communications are active. The interface will be powered off the number of seconds specified by Timeout after the last detected activity.

**NOTE:** Cell interfaces often experience frequent network traffic, which typically prevents them from shutting down. If shutting down a cell interface is desired, setting a short timeout (minimum of one second), in the optional timeout parameter can be used to prompt a shutdown.

Type: Constant
