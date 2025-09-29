# SerialFlush (Clear Characters in Serial Input Buffer)

The SerialFlush function is used to clear any characters in the serial input buffer.

## Syntax

SerialFlush(ComPort)

In the following example, serial data that is received over the datalogger's RS232 port is stored into a string. After 20 characters are received, the remaining serial buffer is cleared using the SerialFlush instruction. SerialInChk is used to monitor the number of characters received.

```
Public InString As String * 25

DataTable (SerialT,True,-1)
Sample (1,InString,String)
EndTable

BeginProg
Scan (5,Sec,3,0)
SerialOpen (ComRS232,115200,0,0,10000)
If SerialInChk(ComRS232) > 20 ThenSerialFlush(ComRS232)
SerialIn (InString,ComRS232,100,0,20)
CallTable (SerialT)
NextScan
EndProg
```

## Remarks

This function clears the buffer and leaves the port open. If the input buffer should be cleared before each execution of [SerialIn](serialin.md), place SerialFlush in the code before the SerialIn instruction.

## Parameter

# ComPort (Communication Port)

The communication port that will be used by the instruction.Note thatbeginning with OS 6.0,control ports C1 and C2 can be configured individually for transmit (Tx) or receive (Rx).Right-click to display a list.

| Alphanumeric | Description                                 |
| ------------ | ------------------------------------------- |
| ComUSB       | USB port of the datalogger                  |
| ComRS232     | RS232 port of the datalogger                |
| Com1         | Datalogger's control ports 1 (TX) & 2 (RX)  |
| ComC1_Tx     | Configures control port 1 for transmit only |
| ComC1_Rx     | Configures control port 1 for receive only  |
| ComC2_Tx     | Configures control port 2 for transmit only |
| ComC2_Rx     | Configures control port 2 for receive only  |
| ComRF        | Integrated RF communication                 |

Type: Constant. For all instructions except SerialOpen, this parameter can also be a variable.
