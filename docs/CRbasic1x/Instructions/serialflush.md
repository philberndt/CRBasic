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

This instruction can also be used along with other serial instructions to set up and control the [SDM-SIO1A or SDM-SIO4A](sdmsio1a_sdmsio4a.md)[or SDM-SIO2R](sdmsio2r.md).

**NOTE:** Beginning with OS 3 in the [SDM-SIO1A and SDMSIO4A](sdmsio1a_sdmsio4a.md), and OS 2 in the [SDM-SIO2R](sdmsio2r.md), the Seiralflush() instruction will purge all information from the data logger and SDM recieve buffers. Prior to these SDM operating systems, SerialFlush cleared both the transmit and receive buffers; it now only clears the receive buffer.

**NOTE:** This instruction runs sequentially from the processing task sequencer, regardless of whether the datalogger is in pipeline or sequential mode.

## Parameter

# ComPort (Communication Port)

The communication port that will be used by the instruction. Right-click to display a list.

| Alphanumeric | Description                                |
| ------------ | ------------------------------------------ |
| ComRS232     | RS232 port of the datalogger               |
| ComME        | Datalogger's CS I/O port; modem enabled    |
| Com310       | Datalogger's CS I/O port; COM310 modem     |
| Com320       | Datalogger's CS I/O port; COM320 modem     |
| ComSDC7      | Datalogger's CS I/O port; SDC 7            |
| ComSDC8      | Datalogger's CS I/O port; SDC 8            |
| ComSDC10     | Datalogger's CS I/O port; SDC10            |
| ComSDC11     | Datalogger's CS I/O port; SDC 11           |
| ComC1        | Datalogger's control ports 1 (TX) & 2 (RX) |
| ComC3        | Datalogger's control ports 3 (TX) & 4 (RX) |
| ComC5        | Datalogger's control ports 5 (TX) & 6 (RX) |
| ComC7        | Datalogger's control ports 7 (TX) & 8 (RX) |

SerialOpen, SerialClose, SerialOut, and SerialOutBlock also support output via SDE pin enabled or any SDC address by using extended ComPorts. Valid extended ComPorts are &H1F through &Hff (31..255, SDC address including mode bits. Note, however, that 31 is SDE pin enabled and 32 through 47 are used to address the SDM-SIO1A). When an extended ComPort is used, the output instructions will turn on SDE or address the CS I/O port with the specified SDC address, delay the amount of time specified by TXDelay, output asynchronously at the specified baud rate in the SerialOpen() instruction, delay the amount of time specified by TXDelay, then reset the CS I/O port.

**NOTE:** The Argos and GOES satellite extended ComPort address is &H41 (65). To specify an SDC address, the most significant bit is the address; e.g., SDC 7 would be &H70 (112).

**NOTE:** When using the ComME ComPort with non-PakBus protocols (PPP, Modbus, DNP3, or generic serial applications), incoming characters can be corrupted by concurrent use of the CS I/O port for SDC communication (e.g., keyboard display).

Type: Constant. For all instructions except SerialOpen, this parameter can also be a variable.
