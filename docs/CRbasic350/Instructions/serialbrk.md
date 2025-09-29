# SerialBrk (Break Signal to Communication Port)

The SerialBrk instruction is used to send a break signal with a specified duration to a serial port.

## Syntax

SerialBrk(COMPort,Break)

In the following example, the SerialBrk instruction is used to wake up a sensor every minute. The sensor subsequently sends data back to the datalogger, which is received and stored by the SerialInRecord instruction.

```
Public InString As String * 100, NBytesReturned

BeginProg
'Open serial port, 9600 baud
SerialOpen (COM1,9600,0,0,104)

Scan(5,Sec,0,0)
If IfTime (0,1,Min) Then
'send break to wake sensor
SerialBrk (COM1,100)
'accept incoming string
SerialInRecord (COM1,InString,0,100,13,NBytesReturned,01)
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
| ComRS232          | RS232 port of the datalogger               |
| Com1              | Datalogger's control ports 1 (TX) & 2 (RX) |
| Com2              | COM2 (TX & RX) port of the datalogger      |
| Com3              | COM3 (TX & RX) port of the datalogger      |
| ComC1_Tx          | Configure control port 1 for transmit      |
| ComC2_Tx          | Configure control port 2 for transmit      |

Type: Constant

# Break

The duration for the break in milliseconds.

Type: Variable or Constant
