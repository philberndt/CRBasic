# SerialOpen (Open Communication Port)

The SerialOpen function is used to set up one of the data loggers ports for communication with a non-PakBus

**Note:** PakBus is a proprietary communication protocol developed by Campbell Scientific to facilitate communications between Campbell Scientific devices. Similar in concept to IP (Internet Protocol), PakBus is a packet-switched network protocol with routing capabilities. A registered trademark of Campbell Scientific, Inc.

device.

## Syntax

```
SerialOpen
```

(ComPort,BaudRate,SerialOpenFormat,TXDelay,BufferSize,,AllowSleep[optional])

In the following example program, a serial string is sent out control port 1 and received back into control port 2.The SerialOpen function is used to prepare the ports for communication.

```
'Declare Public Variables
Public RefTemp, batt_volt
Public Counter
Public OutString as string * 1000
Public InString as string * 1000

'Define Data Tables
DataTable (Test,1,-1)
DataInterval (0,15,Sec,10)
Minimum (1,batt_volt,FP2,0,False)
Sample (1,RefTemp,FP2)
Sample (1,OutString,String)
EndTable

'Main Program
BeginProg

'Set up communication ports to send and receive data
'jumper wire fromC1 to C2
SerialOpen(COMC1_TX,115200,0,0,10000)
SerialOpen(ComC2_Rx,115200,0,0,10000)

Scan (1,Sec,0,0)
PanelTemp (RefTemp,4000)
Battery (Batt_volt)

counter = counter + 1
OutString = "enter output string here " + counter

'Send String over control port C1
SerialOut(COMC1_TX,OutString,"",0,100)

'Receive String over comntrol portC2
SerialIn(InString,ComC2_Rx,100,0,1000)

'Call Output Tables
CallTable Test
NextScan
EndProg
```

## Remarks

When the SerialOpen function is executed, the serial port is "opened" and subsequent textual messages will flow in and out of the port in between PakBus packets. The data are directed away from the terminal mode input based on subsequent [SerialIn](serialin.md) and [SerialOut](serialout.md) functions. This function returns **True** when successful or **False** when unsuccessful.

This instruction can be placed after BeginProg and prior to the Scan instruction (so it is executed only once) or after the Scan instruction (so that it is executed with each scan). Once opened, the serial port will remain open and the datalogger will not go into low power mode until a [SerialClose](serialclose.md) is encountered (note that an exception is format 4, where the port will go to sleep after 40 seconds of inactivity, allowing the datalogger to go into a lower power mode). If a serial port is open, other communication may be prevented from occurring over that port. Note also that keeping the RS232 port open, and thus powered up, will increase power consumption. However, it will also prevent the possibility of the first few incoming characters being lost. Consideration should be given to which is the better tradeoff for the application.

SerialOpen will close an existing PPP connection if it opens the ComPort for a PPP interface on that same ComPort.

[SerialFlush](serialflush.md) can be used to clear the buffer under program control.

When the communications port is set up for PakBus protocol, a combination of TXDelay and BufferSize are used to send serial packets. All ports except COM ports are set for PakBus active, unless set otherwise with SerialOpen. With each packet sent, communication is delayed for the period in TXDelay, and then a packet is sent. (An exception is that PakBus Hello messages are delayed for 4 times the TXDelay.) During communication with some devices it may be necessary to limit the packet size (BufferSize) and add a delay (TXDelay) for communication to be successful. For example, PakBus packets are 1000 bytes. The largest packet that an RF95 can accommodate is 248 bytes. Setting the buffer to 240 would limit the packet size and ensure that the RF95's buffer was not exceeded. A delay (e.g.,1,000,000 us) would ensure that each packet has sufficient time to arrive at its destination before the next packet is transmitted.

It may also be necessary to set up a buffer for the receipt of incoming characters (such as the response to dial commands). If a buffer is needed for both sending and receiving characters, you can perform a SerialOpen/ send data /SerialClose, and then SerialOpen/ receive data /SerialClose.

**NOTE:** This instruction runs inside the processing task.

## Parameters

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

# BaudRate (Data Transmit Rate)

The rate, in bps, at which data is transmitted. The options are 300, 1200, 2400, 4800, 9600, 19200, 38400, 57600, and 115200. Selecting one of these options fixes the baud rate at that rate of communication.

**NOTE:** This datalogger does not support autobaud mode.

**NOTE:** If a serial port is opened, it must be closed before changing the port baud rate.

Right-click this parameter to display a list.

**Type:** Constant. In [SerialOpen](#), BaudRate can be a variable.

# SerialOpenFormat (Error Detection Format)

The Format parameter is used to specify the type of error detection to be used for the exchange of datausing RS-232 logic.

**NOTE:** All formats use one start bit.

| Code | Description                                                                                                           |
| ---- | --------------------------------------------------------------------------------------------------------------------- |
| 0    | No parity, one stop bit, 8 data bits; No error checking; PakBus communication can occur concurrently on the same port |
| 1    | Odd parity, one stop bit, 8 data bits                                                                                 |
| 2    | Even parity, one stop bit, 8 data bits                                                                                |
| 3    | Binary, no parity, one stop bit, 8 data bits                                                                          |
| 4    | PakBus protocol active, 2 stop bits, 8 data bits                                                                      |
| 5    | Odd parity, two stop bits, 8 data bits                                                                                |
| 6    | Even parity, two stop bits, 8 data bits                                                                               |
| 7    | Binary, no parity, two stop bits, 8 data bits                                                                         |
| 9    | Odd parity, one stop bit, 7 data bits                                                                                 |
| 10   | Even parity, one stop bit, 7 data bits                                                                                |
| 11   | Binary, no parity, one stop bit, 7 data bits                                                                          |
| 13   | Odd parity, two stop bits, 7 data bits                                                                                |
| 14   | Even parity, two stop bits, 7 data bits                                                                               |
| 15   | Binary, no parity, two stop bits, 7 data bits                                                                         |

By default, the RS-232 is set up for PakBus communication (i.e., PakBus active), unless changed by SerialOpen. However, the controlport isnot. They must first be set up for PakBus communication using option 4.

**NOTE:** Format 0 ignores ASCII character 0 (Null) and ASCII characters above 127. Format 3 should be used in place of format 0 when handling binary data. Format 0 should be used only for ASCII communication. Non-ASCII characters will be filtered by the PakBus communications stack while searching for a PakBus frame.

When disabling PakBus communication on the RS232 port, an alternative method of connection should be used, such as the ComUSB port.

Type: Constant

# TXDelay (Time to Delay)

Specifies the amount of time to delay, in microseconds, before outputting strings. A delay may be necessary in some situations, such as half duplex RF media.

Type: Variable or Constant

# BufferSize (Buffer Size)

Specifies the number of bytes allocated for input on the ComPort. This buffer is set up as ring memory. Use SerialFlush to clear the buffer.

Note that the datalogger tracks a write pointer and a read pointer for the buffer. It determines if new data has been written by write pointer - read pointer. If the number of incoming bytes stored in the buffer is equal to the buffer size, the datalogger may not detect that new data has been received in the buffer. Therefore, the buffer size should be set to at least the number of expected incoming characters + 1.

ForPakBus

**Note:** PakBus is a proprietary communication protocol developed by Campbell Scientific to facilitate communications between Campbell Scientific devices. Similar in concept to IP (Internet Protocol), PakBus is a packet-switched network protocol with routing capabilities. A registered trademark of Campbell Scientific, Inc.

communication (format option 4, PakBus protocol active), there is a PakBus buffer by default,so BufferSize can normally be left at **0**.

Type: Constant Integer

## Optional Parameter

# AllowSleep (Allow Sleep Mode)

Optional parameteravailable in Operating Systems 7 and laterthat allows the datalogger to go into sleep mode during periods of inactivity.

- ** 0 **or absent = do not allow sleep mode

- ** 1 **or <>** 0 **= allow low-power sleep mode during periods of inactivity

** Caution:**After entering sleep mode, incoming data will wake up the datalogger, but it is possible that the first couple of bytes will be lost or corrupted during the wakeup period.

Type: Constant
