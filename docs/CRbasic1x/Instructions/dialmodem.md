# DialModem

The DialModem function is used to send a modem dial string out one of the datalogger ports.

## Syntax

DialModem(ComPort,BaudRate,DialString,ResponseString)

or

variable = DialModem( ComPort, BaudRate, DialString, ResponseString )

The program below uses the DialModem instruction to call out a modem enabled device at 9600 baud. A variable holds the result of DialModem so that the success/failure result can be used by the EndDialSequence instruction. If the call fails, the link will be terminated at the EndDialSequence instruction. If the call is successful, the device will be kept on-line until the SendVariables command is completed.

```
Public RefT, TCTemp, DialOK, SendResult

DialSequence (4094)
DialOK=DialModem(ComME,9600,"555-1212","")
EndDialSequence (DialOk)

BeginProg
Scan(1,Sec,3,0)
PanelTemp (RefT,15000)
TCDiff (TCTemp,1,mv200C,1,TypeT,RefT,True,0,15000,1.0,0)

If TCTemp > 32 Then
PortSet (C8,1)
SendVariables (SendResult,ComME,4094,4094,0000,6000,"Public","Callback",TCTemp,1)
else
PortSet (C8,0)
endif
NextScan
EndProg
```

## Remarks

The DialModem function performs a [SerialOpen](serialopen.md), multiple [SerialOuts](serialout.md), and a [SerialClose](serialclose.md). This function returns a -1 if the ResponseString is successfully received or a 0 will be returned if it isn't.

DialModem can be used within the [DialSequence/EndDialSequence](dialsequenceenddialsequence.md) commands to specify a communication route to be used for a PakBus device, or it can be used within the BeginProg/EndProg statements to send the dial string any time the instruction is executed. When used within the DialSequence/EndDialSequence commands, use this function as the expression that will be used for the DialSuccess parameter in EndDialSequence. The variable will be monitored by the EndDialSequence instruction. If the call is unsuccessful, the link will be closed.

**NOTE:** This instruction runs sequentially from the processing task sequencer, regardless of whether the datalogger is in pipeline or sequential mode.

## Parameters

# ComPort (Communications Port)

The communications port that will be used by the instruction. Right-click to display a list. Options vary depending on the instruction.

| Alphanumeric | Description                           |
| ------------ | ------------------------------------- |
| ComRS232     | RS232 port of the datalogger          |
| ComME        | Datalogger CS I/O port; modem enabled |
| Com310       | Datalogger CS I/O port; COM310 modem  |
| Com320       | Datalogger CS I/O port, COM320 modem  |
| ComSDC7      | Datalogger CS I/O port; SDC7          |
| ComSDC8      | Datalogger CS I/O port; SDC8          |
| ComSDC10     | Datalogger CS I/O port; SDC10         |
| ComSDC11     | Datalogger CS I/O port; SDC11         |
| ComC1        | Datalogger control terminals 1 & 2    |
| ComC3        | Datalogger control terminals 3 & 4    |
| ComC5        | Datalogger control terminals 5 & 6    |
| ComC7        | Datalogger control terminals 7 & 8    |

Type: Constant. For all instructions except SerialOpen, this parameter can also be a variable.

# BaudRate (Data Transmit Rate)

The rate, in bps, at which data is transmitted. The options are 300,600,1200, 2400, 4800, 9600, 19200, 38400, 57600, and 115200. Selecting one of these options fixes the baud rate at that rate of communication.If a negative baud rate is entered, the first communication attempt will be at the specified baud rate, but if communication fails at that rate, the datalogger will go into autobaud mode (where it will try different rates until successful or until the instruction times out).

**NOTE:** Autobaud is not available on control ports used as com ports. Baud rate for SDC ports must be 9600 or greater.If a serial port is opened, it must be closed before changing the port baud rate.

**NOTE:** If you are using SerialOpen to control a SDM-SIOx (SDM-SI01A, SDM-SIO1A, SDM-SIO2R) automatic baud rate detection is not supported. Rather, setting the baud rate to a negative value enables automatic flow control (RTS/CTS).Click herefor additional information.

Right-click this parameter to display a list.

**Type:** Constant. In [SerialOpen](serialopen.md), BaudRate can be a variable.

# DialString

The telephone number and any other codes used to dial the modem. A semi-colon is used to separate multiple commands. Two semi-colons in a row will insert a 1 second delay before continuing to the next characters in the string. A comma inserts a 2 second delay before continuing to the next character.

Type: String

# ResponseString

Used to specify the response code expected back from the modem when a connection is made. An entry for this parameter is required.

When using the COM220 modem, enter a null string (""). The COM220 can return one of eight responses, depending upon the baud rate at which the connection is made. When a null string is entered, the datalogger will compare the modem's response with these eight known responses in an attempt to establish communication.

Type: String
