# SerialClose (Close Communication Port)

The SerialClose function is used to close a communications port that was previously opened by the [SerialOpen](serialopen.md) function.

## Syntax

```
SerialClose
```

(ComPort)

In the following example, when flag 1 is toggled highCOMs 1 and 3 are opened, allowing a string to be sent from COM1 to COM3. The string that is sent is determined by flags 2 through 5. SerialClose is inserted in the program to close the communication ports so other devices may communicate if necessary.

To test this program in the datalogger, wireC1 to C4 and C2 to C3 (i.e., COM1 TX to COM3 RX and COM1 RX to COM3 TX). The results can be seen on the Numeric Monitor by viewing the Public and SerialT tables.

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

SerialOpen(ComC1,115200,0,0,10000)
SerialOpen(ComC3,115200,0,0,10000)
EndIf

If Flag(2) Then Str ="This is a test"
If Flag(3) Then Str ="Now is the time for all good men to come to the aid of their country"
If Flag(4) Then Str ="Serial In Out Block Test"
If Flag(5) Then Str ="Last String"

SerialOut (ComC1,Str,"",0,0)
SerialIn (InCom2,ComC3,5,-1,1000)
CallTable (SerialT)

SerialClose(ComC1)
SerialClose(ComC3)
NextScan
EndProg

```

## Remarks


This function returns True (-1) if the port was opened or False (0) if it was already closed.

Note that when executed, SerialClose will drop the DTR line on ComRS232or the ME (modem enable) line on ComME.

This instruction can also be used along with other serial instructions to set up and control the [SDM-SIO1A or SDM-SIO4A](sdmsio1a_sdmsio4a.md)[or SDM-SIO2R](sdmsio2r.md).

**NOTE:** This instruction runs sequentially from the processing task sequencer, regardless of whether the datalogger is in pipeline or sequential mode.


## Parameter



# ComPort (Communication Port)


The communication port that will be used by the instruction. Right-click to display a list.

| Alphanumeric | Description |
| --- | --- |
| ComRS232 | RS232 port of the datalogger |
| ComME | Datalogger's CS I/O port; modem enabled |
| Com310 | Datalogger's CS I/O port; COM310 modem |
| Com320 | Datalogger's CS I/O port; COM320 modem |
| ComSDC7 | Datalogger's CS I/O port; SDC 7 |
| ComSDC8 | Datalogger's CS I/O port; SDC 8 |
| ComSDC10 | Datalogger's CS I/O port; SDC10 |
| ComSDC11 | Datalogger's CS I/O port; SDC 11 |
| ComC1 | Datalogger's control ports 1 (TX) & 2 (RX) |
| ComC3 | Datalogger's control ports 3 (TX) & 4 (RX) |
| ComC5 | Datalogger's control ports 5 (TX) & 6 (RX) |
| ComC7 | Datalogger's control ports 7 (TX) & 8 (RX) |

SerialOpen, SerialClose, SerialOut, and SerialOutBlock also support output via SDE pin enabled or any SDC address by using extended ComPorts. Valid extended ComPorts are &H1F through &Hff (31..255, SDC address including mode bits. Note, however, that 31 is SDE pin enabled and 32 through 47 are used to address the SDM-SIO1A). When an extended ComPort is used, the output instructions will turn on SDE or address the CS I/O port with the specified SDC address, delay the amount of time specified by TXDelay, output asynchronously at the specified baud rate in the SerialOpen() instruction, delay the amount of time specified by TXDelay, then reset the CS I/O port.

**NOTE:** The Argos and GOES satellite extended ComPort address is &H41 (65). To specify an SDC address, the most significant bit is the address; e.g., SDC 7 would be &H70 (112).

**NOTE:** When using the ComME ComPort with non-PakBus protocols (PPP, Modbus, DNP3, or generic serial applications), incoming characters can be corrupted by concurrent use of the CS I/O port for SDC communication (e.g., keyboard display).

Type: Constant. For all instructions except SerialOpen, this parameter can also be a variable.
```
