# DialSequence, EndDialSequence

The DialSequence/EndDialSequence instructions are used to define the code necessary to route packets to a PakBus device.

## Syntax

DialSequence(PakBusAddr)

dialing instructions; for example,DialSuccess=[DialModem](dialmodem.md)(ComPort, DialString, ResponseString)

EndDialSequence(DialSuccess)

The program below uses the DialSequence/EndDialSequence instructions to define the dial-out string to reach PakBus device 4094 (in this case, a PC running support software). Each time the program executes the SendVariables command, the dial sequence code is executed.
A variable holds the result of DialModem so that the success/failure result can be used by the EndDialSequence instruction. If the call fails, the link will be terminated at the EndDialSequence instruction. If the call is successful, the device will be kept on-line until the SendVariables command is completed.

```
Public RefT, TCTemp, DialOK, SendResult

DialSequence(4094)
DialOK=DialModem(ComME,9600,"555-1212","")
EndDialSequence(DialOk)

BeginProg
Scan(1,Sec,3,0)
PanelTemp (RefT,15000)
TCDiff (TCTemp,1,mv200C,1,TypeT,RefT,True,0,15000,1.0,0)

If TCTemp > 32 Then
SendVariables(SendResult,ComME,4094,4094,0000,6000,"Public","Callback",TCTemp,1)
Else
PortSet(C4,0)
EndIf
NextScan
EndProg
```

## Remarks

The PakBus device can be another datalogger in the PakBus network, a computer running PakBus capable software, or any other device which uses a PakBus address. The DialSequence instruction indicates the beginning of the code; the EndDialSequence indicates the ending. The code is entered in the declarations section of the program, prior to the main program (defined by the BeginProg/EndProg instructions).

Any time an instruction in the main program requires that communication be made with the remote PakBus device identified by the PakBusAddr parameter, and the route is not known or the COM port of the route is closed, the DialSequence code for that device will be executed. The code will also be executed if the datalogger receives a message from another PakBus device that needs to be routed to the remote device but the route is not known or the COM port for the route is closed.

**NOTE:** This instruction runs sequentially from the processing task sequencer, regardless of whether the datalogger is in pipeline or sequential mode.

## Parameters

# PakBusAddr (PakBus Address)

ThePakbus

**Note:** PakBus is a proprietary communication protocol developed by Campbell Scientific to facilitate communications between Campbell Scientific devices. Similar in concept to IP (Internet Protocol), PakBus is a packet-switched network protocol with routing capabilities. A registered trademark of Campbell Scientific, Inc.

address of the device that will be contacted as a result of this instruction. Each PakBus device in the network must have a unique address.

If PakBus encryption is enabled in the datalogger, by default the datalogger will encrypt the communications sent using PakBus communication. To disable encryption for one or more Pakbus addresses, use the EncryptExempt instruction. This is useful if a remote device does not support encryption (such as a CR200 or an AVW200).

Type: Integer between 1 and 4094

**NOTE:** By default,Campbell Scientificsoftware uses the following PakBus addresses: LoggerNet 4094, VisualWeather 4094, PC400 4093, PC200W 4092, PConnect/PConnectCE 4091, RTDAQ 4090, Device Configuration Utility 4089, Konect GDS a value in the range of 4000 4050. 4095 is a broadcast address that can be used in a limited number of instructions.

# DialSuccess

A variable that will be monitored for the success/failure of the communication attempt. If the communication attempt fails, the communication link will be closed. A variable holding the result of [DialModem](dialmodem.md) can be used for this parameter.

Type: Variable
