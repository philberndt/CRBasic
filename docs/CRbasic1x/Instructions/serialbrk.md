# SerialBrk (Break Signal to Communication Port)

The SerialBrk instruction is used to send a break signal with a specified duration to a serial port.

## Syntax

SerialBrk(COMPort,Break)

In the following example, the SerialBrk instruction is used to wake up a sensor every minute. The sensor subsequently sends data back to the datalogger, which is received and stored by the SerialInRecord instruction.

```
Public InString As String * 100, NBytesReturned

BeginProg
'Open serial port, 9600 baud
SerialOpen (COMC1,9600,0,0,104)

Scan(5,Sec,0,0)
If IfTime (0,1,Min) Then
'send break to wake sensor
SerialBrk (COMC1,100)
'accept incoming string
SerialInRecord (COMC1,InString,0,100,13,NBytesReturned,01)
EndIf
NextScan
EndProg
```

## Remarks

SerialBrk puts the TX line of a COMPort into a break condition for the specified Break time. This instruction runs in the processing task.

## Parameters

# ComPort (Communication Port)

Specifies the communication port to which the break will be sent.

| Alphanumeric Code | Description                                |
| ----------------- | ------------------------------------------ |
| ComC1             | Datalogger's control ports 1 (TX) & 2 (RX) |
| ComC3             | Datalogger's control ports 3 (TX) & 4 (RX) |
| ComC5             | Datalogger's control ports 5 (TX) & 6 (RX) |
| ComC7             | Datalogger's control ports 7 (TX) & 8 (RX) |

Type: Constant

# Break

The duration for the break in milliseconds.

Type: Variable or Constant
