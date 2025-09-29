# SerialClose (Close Communication Port)

The SerialClose function is used to close a communications port that was previously opened by the [SerialOpen](serialopen.md) function.

## Syntax

```
SerialClose
```

(ComPort)

In the following example, when flag 1 is toggled highCOMC1_TX and COMC2_RX are opened allowing a string to be sent from control port 1 to control port 2. The string that is sent is determined by flags 2 through 5. SerialClose is inserted in the program to close the communication ports so other devices may communicate if necessary.

To test this program in the datalogger, wireC1 to C2. The results can be seen on the Numeric Monitor by viewing the Public and SerialT tables.

```
SequentialMode
Dim Str As STRING * 1000
Public InCom2 As String * 1000
Public Flag(5)

DataTable (SerialT,True,-1)
Sample (1,Str,String)
Sample (1,InCom2,String)
EndTable

BeginProg
Scan (5,Sec,3,0)
If Flag(1) Then
```

SerialOpen(COMC1_TX,115200,0,0,10000)
SerialOpen(ComC2_Rx,115200,0,0,10000)
EndIf

If Flag(2) Then Str ="This is a test"
If Flag(3) Then Str ="Now is the time for all good men to come to the aid of their country"
If Flag(4) Then Str ="Serial In Out Block Test"
If Flag(5) Then Str ="Last String"

SerialOut (COMC1_TX,Str,"",0,0)
SerialIn (InCom2,ComC2_Rx,5,-1,1000)
CallTable (SerialT)

SerialClose(COMC1_TX)
SerialClose(ComC2_Rx)
NextScan
EndProg

```

## Remarks


This function returns True (-1) if the port was opened or False (0) if it was already closed.

Note that when executed, SerialClose will drop the DTR line on ComRS232.


## Parameter



# ComPort (Communication Port)


The communication port that will be used by the instruction.Note thatbeginning with OS 6.0,control ports C1 and C2 can be configured individually for transmit (Tx) or receive (Rx).Right-click to display a list.

| Alphanumeric | Description |
| --- | --- |
| ComUSB | USB port of the datalogger |
| ComRS232 | RS232 port of the datalogger |
| Com1 | Datalogger's control ports 1 (TX) & 2 (RX) |
| ComC1\_Tx | Configures control port 1 for transmit only |
| ComC1\_Rx | Configures control port 1 for receive only |
| ComC2\_Tx | Configures control port 2 for transmit only |
| ComC2\_Rx | Configures control port 2 for receive only |
| ComRF | Integrated RF communication |

Type: Constant. For all instructions except SerialOpen, this parameter can also be a variable.
```
