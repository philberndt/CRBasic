# EthernetPower (Power Ethernet)

The EthernetPower instruction is used to turn the power to all Ethernet devices on or off. To control power for a single IP network device, use [IPNetPower](ipnetpower.md).

## Syntax

EthernetPower(State)

The following program shows the use of the EthernetPower instruction to turn on power to the Ethernet only when an email is being sent.

Note that this is a test program only to show the functionalilty of the EthernetPower instruction. In a real-world scenario you would likely want to handle retries in case of failure.

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
Scan (1,Sec,3,0)
Battery (Batt)
PanelTemp (RefTemp,15000)
TCDiff (Temp,1,mv200C,1,TypeT,RefTemp,True,0,15000,1.0,0)

EthernetPower(Estate)
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
Message = Message + "The temperature is " + Temp + " degrees C." + CRLF + CRLF + CRLF
Message = Message + "Datalogger time is " + Status.Timestamp
EmailSuccess=EMailSend (ServerAddr,ToAddr,FromAddr,Subject,Message,Attach,UserName,Password,Result)
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

EthernetPower controls power to all Ethernet networks within the datalogger (Ethernet, WiFi, NL200, NL240, internal cellular modem, etc.). The State variable determines whether the power is on or off. A non-zero value turns the power on; zero turns the power off. By default, Ethernet power is ON unless turned off.

To control pwer for a single Ethernet device, use the IPNetPower instruction.
