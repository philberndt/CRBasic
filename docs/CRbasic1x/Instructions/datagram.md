# DataGram (Act as PakBus Server)

The DataGram instruction is used to initialize a SerialServer/DataGram/PakBus

**Note:** PakBus is a proprietary communication protocol developed by Campbell Scientific to facilitate communications between Campbell Scientific devices. Similar in concept to IP (Internet Protocol), PakBus is a packet-switched network protocol with routing capabilities. A registered trademark of Campbell Scientific, Inc.

application in the datalogger when a program is compiled.

## Syntax

DataGram(ComPort,BaudRate,PakBusAddr,DestAppID,SrcAppID)

The following two programs are an example of twoCR1000Xs using the DataGram instruction to route data between a LoggerNet server and a CR10X datalogger. These programs would also be applicable for other dataloggers.

```
'ForCR1000X#1 (near LN) with PBA = 1 acting as a PakBus Serial Server for LN JK commands to/from CR10X.
'CR1000X_1's Destination ID isCR1000X_2's Source ID = 20
'CR1000X_1's Source ID is 10
'Pakbus Serial Server will output on ComRS232 packets received from Destination ID 20.
'Pakbus Serial Server will packetize and output toward Destination ID 20 any JK cmds received on CoMRS232.
'A wired ComC1 link is established between CR1000X's to eliminate rf interference.
'Hardwire link discovery is set up with Neighbors Allowed ComC1= (2,2) and Verify interval ComC1= 30.
'BothCR1000Xs are configured as routers.

Public PTemp, batt_volt

DataTable (Test1,1,-1)
DataInterval (0,15,Sec,10)
Minimum (1,batt_volt,FP2,0,False)
Sample (1,PTemp,FP2)
EndTable

BeginProg
SerialOpen (COMC1,9600,0,0,10000)

Scan (1,Sec,0,0)
PanelTemp (PTemp,15000)
Battery (Batt_volt)
DataGram(ComRS232,9600,2,20,10)
'Source and Destination IDs are only ID numbers, not PakBus addresses.
CallTable Test1
NextScan
EndProg
'//////////////////End of First Program\\\\\\\\\\\\\\\\\\

'ForCR1000X#2 (near CR10X) with PBA = 2 acting as a PakBus Serial Server for CR10X JK commands to/from LN.
'CR1000X_2's Destination ID isCR1000X_2's Source ID = 10
'CR1000X_2's Source ID is 20
'Pakbus Serial Server will output on ComSDC7 packets received from Destination ID 10.
'Pakbus Serial Server will packetize and output toward Destination ID 10 any JK cmds received on COMSDC7.
'A wired COMC3link is established betweenCR1000Xs to eliminate rf interference.
'Hardwire link discovery is set up with BeaconComC3=30 (sec) and bothCR1000Xs are configured as routers.

Public PTemp, batt_volt

DataTable (Test2,1,-1)
DataInterval (0,15,Sec,10)
Minimum (1,batt_volt,FP2,0,False)
Sample (1,PTemp,FP2)
EndTable

BeginProg
SerialOpen (COMC3,9600,0,0,10000)

Scan (1,Sec,0,0)
PanelTemp (PTemp,15000)
Battery (Batt_volt)
DataGram(ComSDC7,9600,2,10,20)
'Source and Destination IDs are only ID numbers, not PakBus addresses.
CallTable Test2
NextScan
EndProg
```

## Remarks

The DataGram instruction enables the datalogger to act as a PakBus serial server, routing non-PakBus messages in a PakBus Network over a specified port using the PakBus Datagram Protocol. The PakBus Datagram Protocol extends PakBus communication for general application use, including sending commands to mixed array dataloggers orModbus

**Note:** Communication protocol published by Modicon in 1979 for use in programmable logic controllers (PLCs).

devices.

A PakBus datagram consists of a PakBus header (which includes an address for the destination PakBus device), a Destination Application Identifier (DestAppID), a Source Application Identifier (SrcAppID), and the body of the message. The PakBus header is used to route the datagram to the specified PakBus device. This device may be running one or more applications that use the datagram protocol. The DestAppID is used to route the datagram to the appropriate application within the PakBus device. The SrcAppID registers the datalogger in the network so that datagrams can be passed back to it using the PakBus Datagram Protocol.

In networks using the DataGram instruction to communicate with a non-PakBus datalogger, extra response time may be needed in the software setup to allow for the time needed to transfer packets.

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

For the DataGram instruction, a variable can be used in the ComPort parameter for use with functions that return a communication port. If communication occurs usingTCP/IP

**Note:** Transmission Control Protocol / Internet Protocol.

, enter the variable for the socket returned by [TCPOpen](tcpopen.md).

# BaudRate (Data Transmit Rate)

The rate, in bps, at which data is transmitted. The options are 300,600,1200, 2400, 4800, 9600, 19200, 38400, 57600, and 115200. Selecting one of these options fixes the baud rate at that rate of communication.If a negative baud rate is entered, the first communication attempt will be at the specified baud rate, but if communication fails at that rate, the datalogger will go into autobaud mode (where it will try different rates until successful or until the instruction times out).

**NOTE:** Autobaud is not available on control ports used as com ports. Baud rate for SDC ports must be 9600 or greater.If a serial port is opened, it must be closed before changing the port baud rate.

**NOTE:** If you are using SerialOpen to control a SDM-SIOx (SDM-SI01A, SDM-SIO1A, SDM-SIO2R) automatic baud rate detection is not supported. Rather, setting the baud rate to a negative value enables automatic flow control (RTS/CTS).Click herefor additional information.

Right-click this parameter to display a list.

**Type:** Constant. In [SerialOpen](serialopen.md), BaudRate can be a variable.

# PakBusAddr (PakBus Address)

ThePakbus

**Note:** PakBus is a proprietary communication protocol developed by Campbell Scientific to facilitate communications between Campbell Scientific devices. Similar in concept to IP (Internet Protocol), PakBus is a packet-switched network protocol with routing capabilities. A registered trademark of Campbell Scientific, Inc.

address of the device that will be contacted as a result of this instruction. Each PakBus device in the network must have a unique address.

If PakBus encryption is enabled in the datalogger, by default the datalogger will encrypt the communications sent using PakBus communication. To disable encryption for one or more Pakbus addresses, use the EncryptExempt instruction. This is useful if a remote device does not support encryption (such as a CR200 or an AVW200).

Type: Integer between 1 and 4094

**NOTE:** By default,Campbell Scientificsoftware uses the following PakBus addresses: LoggerNet 4094, VisualWeather 4094, PC400 4093, PC200W 4092, PConnect/PConnectCE 4091, RTDAQ 4090, Device Configuration Utility 4089, Konect GDS a value in the range of 4000 4050. 4095 is a broadcast address that can be used in a limited number of instructions.

# DestAppID (Destination Device ID)

An application ID number that the user assigns to the destination device. The number can be any value from 0 to 65535.

**NOTE:** Application IDs 0 through 999 are reserved for common applications (for instance, 502 is reserved forModbus

**Note:** Communication protocol published by Modicon in 1979 for use in programmable logic controllers (PLCs).

protocol). Therefore, a user application ID should be in the range of 1000 to 65535.

Type: Integer

# SrcAppID (Source Device ID)

An application ID number that the user assigns to the source device, which will be used to identify the application in thePakBus

**Note:** PakBus is a proprietary communication protocol developed by Campbell Scientific to facilitate communications between Campbell Scientific devices. Similar in concept to IP (Internet Protocol), PakBus is a packet-switched network protocol with routing capabilities. A registered trademark of Campbell Scientific, Inc.

network. The number can be any value from 0 to 65535.

**NOTE:** Application IDs 0 through 999 are reserved for common applications (for instance, 502 is reserved forModbus

**Note:** Communication protocol published by Modicon in 1979 for use in programmable logic controllers (PLCs).

protocol). Therefore, a user application ID should be in the range of 1000 to 65535.

Type: Integer
