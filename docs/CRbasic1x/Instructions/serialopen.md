# SerialOpen (Open Communication Port)

The SerialOpen function is used to set up one of the data loggers ports for communication with a non-PakBus

**Note:** PakBus is a proprietary communication protocol developed by Campbell Scientific to facilitate communications between Campbell Scientific devices. Similar in concept to IP (Internet Protocol), PakBus is a packet-switched network protocol with routing capabilities. A registered trademark of Campbell Scientific, Inc.

device.

This instruction can also be used along with other serial instructions to set up and control the [SDM-SIO1A or SDM-SIO4A or SDM-SIO2R](sdmsio1a_sdmsio4a.md).

## Syntax

```
SerialOpen
```

(ComPort,BaudRate,SerialOpenFormat,TXDelay,BufferSize,CommsMode[optional])

In the following example program, a serial string is sent out COM port 1 (control port 1) and received back into COM port 3 (control port 4).The SerialOpen function is used to prepare the ports for communication.

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
'jumper wire fromC1 (COM1 TX) to C4 (COM3 RX)
SerialOpen(ComC1,115200,0,0,10000)
SerialOpen(ComC3,115200,0,0,10000)

Scan (1,Sec,0,0)
PanelTemp (RefTemp,15000)
Battery (Batt_volt)

counter = counter + 1
OutString = "enter output string here " + counter

'Send String over control port C1(COM1 TX).
SerialOut(ComC1,OutString,"",0,100)

'Receive String over comntrol portC4 (COM3 RX).
SerialIn(InString,ComC3,100,0,1000)

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

# BaudRate (Data Transmit Rate)

The rate, in bps, at which data is transmitted. The options are 300,600,1200, 2400, 4800, 9600, 19200, 38400, 57600, and 115200. Selecting one of these options fixes the baud rate at that rate of communication.If a negative baud rate is entered, the first communication attempt will be at the specified baud rate, but if communication fails at that rate, the datalogger will go into autobaud mode (where it will try different rates until successful or until the instruction times out).

**NOTE:** Autobaud is not available on control ports used as com ports. Baud rate for SDC ports must be 9600 or greater.If a serial port is opened, it must be closed before changing the port baud rate.

**NOTE:** If you are using SerialOpen to control a SDM-SIOx (SDM-SI01A, SDM-SIO1A, SDM-SIO2R) automatic baud rate detection is not supported. Rather, setting the baud rate to a negative value enables automatic flow control (RTS/CTS).Click herefor additional information.

Right-click this parameter to display a list.

**Type:** Constant. In [SerialOpen](#), BaudRate can be a variable.

# SerialOpenFormat (Error Detection Format)

The Format parameter is used to specify the type of error detection to be used for the exchange of data.These format options apply to all ComPorts except COM310 and COMSDC ports, as the format is ignored for these ports due to their predefined behavior.

If you are using SerialOpen to control a **SDM-SIO1A, SDM-SIO4A, or SDM-SIO2R** module,click herefor details on SerialOpen format parameters that are specific for SDM-SIO devices.

**NOTE:** Typical RS-232 connections use logic 1 low. Typical TTL connections use logic 1 high.

**NOTE:** With RS-485 and RS-422 communications, logic level is ignored. You must select the parity, stop, and data bits. However, either logic level can be used.

**NOTE:** All formats use one start bit.

| Code | Description                                                                                                                         |
| ---- | ----------------------------------------------------------------------------------------------------------------------------------- | --------------------- | ---- | ----------- |
| 0    | Logic 1 low;No parity, one stop bit, 8 data bits; No error checking; PakBus communication can occur concurrently on the same port   |
| 1    | Logic 1 low;Odd parity, one stop bit, 8 data bits                                                                                   |
| 2    | Logic 1 low;Even parity, one stop bit, 8 data bits                                                                                  |
| 3    | Logic 1 low;Binary, no parity, one stop bit, 8 data bits                                                                            |
| 4    | Logic 1 low;PakBus protocol active, 2 stop bits, 8 data bits                                                                        |
| 5    | Logic 1 low;Odd parity, two stop bits, 8 data bits                                                                                  |
| 6    | Logic 1 low;Even parity, two stop bits, 8 data bits                                                                                 |
| 7    | Logic 1 low;Binary, no parity, two stop bits, 8 data bits                                                                           |
| 9    | Logic 1 low;Odd parity, one stop bit, 7 data bits                                                                                   |
| 10   | Logic 1 low;Even parity, one stop bit, 7 data bits                                                                                  |
| 11   | Logic 1 low;Binary, no parity, one stop bit, 7 data bits                                                                            |
| 13   | Logic 1 low;Odd parity, two stop bits, 7 data bits                                                                                  |
| 14   | Logic 1 low;Even parity, two stop bits, 7 data bits                                                                                 |
| 15   | Logic 1 low;Binary, no parity, two stop bits, 7 data bits                                                                           | **C Terminals Only ** | Code | Description |
| ---  | ---                                                                                                                                 |
| 16   | Logic 1 high; No parity, one stop bit, 8 data bits; No error checking; PakBus communication can occur concurrently on the same port |
| 17   | Logic 1 high; Odd parity, one stop bit, 8 data bits                                                                                 |
| 18   | Logic 1 high; Even parity, one stop bit, 8 data bits                                                                                |
| 19   | Logic 1 high; Binary, no parity, one stop bit, 8 data bits                                                                          |
| 20   | Logic 1 high; PakBus protocol active, 2 stop bits, 8 data bits                                                                      |
| 21   | Logic 1 high; Odd parity, two stop bits, 8 data bits                                                                                |
| 22   | Logic 1 high; Even parity, two stop bits, 8 data bits                                                                               |
| 23   | Logic 1 high; Binary, no parity, two stop bits, 8 data bits                                                                         |
| 25   | Logic 1 high; Odd parity, one stop bit, 7 data bits                                                                                 |
| 26   | Logic 1 high; Even parity, one stop bit, 7 data bits                                                                                |
| 27   | Logic 1 high; Binary, no parity, one stop bit, 7 data bits                                                                          |
| 29   | Logic 1 high; Odd parity, two stop bits, 7 data bits                                                                                |
| 30   | Logic 1 high; Even parity, two stop bits, 7 data bits                                                                               |
| 31   | Logic 1 high; Binary, no parity, two stop bits, 7 data bits                                                                         |

By default, the RS-232 is set up for PakBus communication (i.e., PakBus active), unless changed by SerialOpen. However, the controlports arenot. They must first be set up for PakBus communication using option 4.

** NOTE:**Formats 0 and 16 ignores ASCII character 0 (Null) and ASCII characters above 127. Formats 3 and 19 should be used in place of formats 0 and 16 respectively, when handling binary data. Formats 0 and 16 should be used only for ASCII communication. Non-ASCII characters will be filtered by the PakBus communications stack while searching for a PakBus frame.

When disabling PakBus communication on the RS232 port, an alternative method of connection should be used, such as the ComUSB port.

Type: Constant

# TXDelay (Time to Delay)

Specifies the amount of time to delay, in microseconds, before outputting strings. A delay may be necessary in some situations, such as half duplex RF media.

When communicating with an SDC device on a port other than SDC7 or SDC8, the TXDelay parameter applies a delay both after addressing the SDC device and before resetting the SDC device.

Type: Variable or Constant

# BufferSize (Buffer Size)

Specifies the number of bytes allocated for input on the ComPort. This buffer is set up as ring memory. Use SerialFlush to clear the buffer.

Note that the datalogger tracks a write pointer and a read pointer for the buffer. It determines if new data has been written by write pointer - read pointer. If the number of incoming bytes stored in the buffer is equal to the buffer size, the datalogger may not detect that new data has been received in the buffer. Therefore, the buffer size should be set to at least the number of expected incoming characters + 1.

ForPakBus

** Note:**PakBus is a proprietary communication protocol developed by Campbell Scientific to facilitate communications between Campbell Scientific devices. Similar in concept to IP (Internet Protocol), PakBus is a packet-switched network protocol with routing capabilities. A registered trademark of Campbell Scientific, Inc.

communication (format option 4, PakBus protocol active), there is a PakBus buffer by default,so BufferSize can normally be left at ** 0**.

Type: Constant Integer

## Optional Parameter

# CommsMode

This optional parameter is left blank when used with the [SDM-SIO1A or SDM-SIO4A](sdmsio1a_sdmsio4a.md). The CommsMode specifies the configuration of the datalogger's control port used by this instruction. The ports can also be configured using Device Configuration Utility. Configuring the ports using the SerialOpen instruction will change the setting if it was previously set by other means.

| Code                                                                                                                                                                                                                                                                                                                                                 | Description                                                                                                                                                                                      |
| ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| 01                                                                                                                                                                                                                                                                                                                                                   | Configures the port as RS-232, 5V voltage levels (ComC1-ComC7 on CR1000Xe; ComC5-ComC7 on CR1000X; and CPI/RS-232)                                                                               |
| 1                                                                                                                                                                                                                                                                                                                                                    | Configures the port as TTL, 5V and 0V voltage levels (ComC1-ComC7)                                                                                                                               |
| 2                                                                                                                                                                                                                                                                                                                                                    | Configures the port as LVTTL, 3.3V and 0V voltage levels (ComC1-ComC7)                                                                                                                           |
| 3                                                                                                                                                                                                                                                                                                                                                    | Configures the port as RS-485 half-duplex, PakBus communication (ComC1, ComC3, ComC5, or ComC7;Odd ports = A-; Even ports = B+)                                                                  |
| 4                                                                                                                                                                                                                                                                                                                                                    | Configures the port as RS-485 half-duplex, transparent (ComC1-ComC7only; Odd ports = A-; Even ports = B+)                                                                                        |
| 52                                                                                                                                                                                                                                                                                                                                                   | Configures the port as RS-485 full-duplex(ComC1 or ComC5only; requires 4 adjacent control portsFor ComC1, C1 = TX-, C2 = TX+, C3 = RX-, C4 = RX+). For ComC5,C5 = TX-,C6= TX+,C7 = RX-,C8 = RX+) |
| 63                                                                                                                                                                                                                                                                                                                                                   | RS-485 transparent, TX only. In this configuration the transceiver is left on so the A(-) and B(+) ports idle in a marking state (logical '1').                                                  |
| 72                                                                                                                                                                                                                                                                                                                                                   | RS-422 full-duplex transparent. In this configuration, the transceiver for the transmit pair is left on. The transmit pair A(-) and B(+) idle in a marking state (logical '1').                  |
| 1Only compatible with ComC5 and ComC7 for the CR1000X model. Compatible with ComC1-ComC7 on CR1000Xe model. 2Only compatible with ComC5 for the CR1000X model. Both ComC1 and Com C5 are compatible on the CR1000Xe model. 3Compatible with ComC5 or ComC7 for the CR1000X model. Compatible with ComC1, ComC3, ComC5, and ComC7 on CR1000Xe models. |                                                                                                                                                                                                  |

** NOTE:**Codes 6 and 7 are available on CR1000Xe data loggers and CR1000X models with OS versions later than v.7.02.

** TIP:**RS-422 protocol is similar to RS-485. Most RS-422 sensors will work with RS-485 protocol.
