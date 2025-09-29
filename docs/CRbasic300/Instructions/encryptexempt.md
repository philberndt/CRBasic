# EncryptExempt (Exempt from Encrypted Messages)

The EncryptExempt instruction is used to define one or morePakBus

**Note:** PakBus is a proprietary communication protocol developed by Campbell Scientific to facilitate communications between Campbell Scientific devices. Similar in concept to IP (Internet Protocol), PakBus is a packet-switched network protocol with routing capabilities. A registered trademark of Campbell Scientific, Inc.

addresses to which the datalogger will not send encrypted PakBus messages, even though PakBus encryption is enabled. This is useful in networks where encryption is used but some devices in the network do not support PakBus encryption (such as a CR200 or AVW200).

## Syntax

EncryptExempt(BeginPakBusAddr,EndPakBusAddr)

The following is a simple program showing the use of the EncryptExempt instruction to make an AVW206 exempt from PakBus encryption.

```
'Declare Public Variables
Public SVResult, GVResult
Public Remote_BattV
Public AVW_Signal
Public Flag(2)

'Main program
EncryptExempt(200,200)'Exempt AVW206 from PakBus encryption
BeginProg
Scan(1,sec,0,0)
If Flag(1) Then
```

'Send encrypted variable to datalogger at address 100
SendVariables (SVResult,ComRS232,100,100,0,0,"Public","BattV",Remote_BattV,1)
EndIF
If Flag(2) Then
'Get AVW206 RF signal level (unencrypted)
GetVariables (GVResult,ComRS232,200,200,0,100,"Status","RfSignalLevel",AVW_signal,1)
EndIf
NextScan
EndProg

```

## Remarks


The datalogger can be configured to send all PakBus communication encrypted using the 128-bit Advanced Encryption Standard (AES-128). If there are PakBus devices the datalogger needs to communicate with that do not support PakBus encryption (such as a CR200 or an AVW200), encryption can be disabled for those devices.This is accomplished by setting a range of PakBus addresses that are exempt from PakBus encryption using this instruction.

The BeginPakBusAddr and EndPakBusAddr parameters are used to define the range. More than one EncryptExempt instruction can be used to define multiple ranges of PakBus addresses.

Example:

'Do not use encryption when communicating with PakBus addresses 15, 16, 17, 18, 19, 20, and 25

```

EncryptExempt ( 15, 20 )

```
EncryptExempt ( 25, 25 )
```

EncryptExempt is a declaration instruction which contains no run-time code. It should be placed before the BeginProg statement.
