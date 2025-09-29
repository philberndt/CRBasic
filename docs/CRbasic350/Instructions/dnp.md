# DNP

TheDNP

**Note:** Distributed Network Protocol is a set of communications protocols used between components in process automation systems. Its main use is in utilities such as electric and water companies.

instruction sets up a datalogger as a DNP outstation device.

## Syntax

DNP(ComPort,BaudRate,Confirmation,TimeOffset[optional],MaxTimeDiff[optional],DNPTLS[optional] )

Example 1. This simple example demonstrates setting up a data logger as a DNP outstation with DNP address 1. The data logger panel temperature and battery voltage are sent to a DNP master with address 10.

```
Public PTemp, batt_volt
Public DNP3_Data(2) As Long'Declare array for DNP3 Data

'Main Program
BeginProg
DNP(20000,115200,1000)'Use port 20000 for Ethernet comms
DNPVariable(DNP3_Data(),2,30,1,0,0,0,0)'DNPVariable instruction for static data
DNPVariable(DNP3_Data(),2,32,1,1,0,0,10)'DNPVariable instruction for event data

Scan (30,Sec,0,0)
PanelTemp (PTemp,4000)
Battery (batt_volt)
```

DNP3_Data(1) = PTemp
DNP3_Data(2) = batt_volt

DNPUpdate(1,10)'Slave Address of 1 and Master Address of 10
NextScan
EndProg

```
'///////////////END OF PROGRAM 1\\\\\\\\\\\\\\\\\\\
```

Example 2. This program sets up a DNP3 array of 2 containing Panel Temperature in the first index and Battery Voltage in the second index. Events are triggered if the panel temperature is above 25 C or if the battery voltage drops below 11.5 volts.

```
Public PTemp, batt_volt

Public DNP3_Data(2) As Long'Declare array for DNP3 Data
Public DNP3_Flag(2) As Long'Declare array for DNP3 Data Quality Flags
Public Event_Trigger(2) As Boolean 'Declare event trigger

'Main Program
BeginProg
DNP(20000,115200,1000)
DNPVariable(DNP3_Data(),2,30,1,0,DNP3_Flag(),0,0)'DNPVariable instruction for static data
DNPVariable(DNP3_Data(),2,32,1,1,DNP3_Flag(),Event_Trigger(),10)'DNPVariable instruction for event data
DNP3_Flag(1) = 1'initialize data quality flag for Ptemp to be "online"
DNP3_Flag(2) = 1'initialize data quality flag for Battery voltage to be "online"

Scan (1,Sec,0,0)
PanelTemp (PTemp,4000)
Battery (batt_volt)

If PTemp > 25 Then'Trigger change event if Ptemp > 25
Event_Trigger(1) = True
Else
Event_Trigger(1) = False
EndIf

If batt_volt < 11.5 Then'Trigger change event if batt_volt < 11.5
Event_Trigger(2) = True
Else
Event_Trigger(2) = False
EndIf

DNP3_Data(1) = PTemp * 100'Scaling by 100
DNP3_Data(2) = batt_volt * 100'Scaling by 100

DNPUpdate(1,10)'Slave Address of 1 and Master Address of 10
NextScan
EndProg

'///////////////END OF PROGRAM 2\\\\\\\\\\\\\\\\\\\
```

Example 3. The following program example demonstrates using analog output object 41 to set an analog value and object 40 to report the status of an analog output point. In this case, a counter is used for demonstration.

```
'DNP3 Program for Analog Outputs
'Declare DNPvariable types ot match the DNP variation
'Var 1 = 32 bit
'Var 2 + 16 bit
'Var 3 = floating point

Public Link_Conf As Long
Public Batt_volt
Public Obj40Var As Long'Use type Float for 41 variation 3; se Long for other variations
Alias Obj40Var = Stage_height
Public Obj40Var_Flag as Long
Public Counter'To generate data for demonstration

BeginProg
Link_Conf = 1000'Disables datalink confirmation if using serial comms
Obj40Var_fLAG = 1
DNPVariable(Obj40Var,1,40,1,0,Obj40Var_Flag(),0,0)'Make variations for objects 40 and 41 the same
DNPVariable(Obj40Var,1,41,1,0,Obj40Var_Flag(),0,0)'Object 41 is used for writing value, 40 is used to read it back out
Scan (1,Sec,0,0)
Counter += 1'Increment counter each scan
Stage_height = Counter
'DNP(ComC1,115200,Link_Conf) 'Serial Example
DNP(20000,115200,Link_Conf)'IP Example
Battery (Batt_volt)
DNPUpdate(1,10)'Slave Address of 1 and Master Address of 10
NextScan
EndProg
'////////////////End Of Program 3\\\\\\\\\\\\\\\\\
```

Example 4. Send Unsolicited Response from Datalogger 1 via TCP/IP using optional parameter, ConnectHandle, in DNPUpdate().:

```
Public Binput (2) As Boolean
Public counter As Long
```

Public IP_Socket As Long
Const = IPAddress ="192.168.xx.xx"
BeginProg
DNP(20000,115200,1000)
DNPVariable(Binput(),2,1,2,0,0,0,0)
DNPVariable(Binput(),2,2,1,1,0,0,2)
Scan(1,Sec,1,0)
IP_Socket = TCPOpen(IPAddress,20000,0)
counter = counter +1
DNPUpdate(1,10,5,0,IP_Socket)
NextScan
EndProg

```

## Remarks


This instruction sets up a datalogger COM port to service DNP commands.


## Parameters



# ComPort (Communications Port)


The communications port that will be used by the instruction. Right-click to display a list. Options vary depending on the instruction.

| Alphanumeric | Description |
| --- | --- |
| ComUSB | USB port of the datalogger |
| ComRS232 | RS232 port of the datalogger |
| Com1 | Datalogger control terminals 1 (TX) & 2 (RX) |
| Com2 | COM2 port of the datalogger |
| Com3 | COM3 port of the datalogger |
| ComRF | Integrated RF communication |

Type: Constant. For all instructions except SerialOpen, this parameter can also be a variable.

**DNP PakBus datagram routing **- DNP datagram allows remote DNP outstation devices to talk with a DNP Master without being directly connected to the Master. In order to implement DNP datagram, the ComPort parameter on a router datalogger is preceded with a minus sign (for example, -ComRS232) and the comport parameter on the remote datalogger is set to 0. You must use a different COM port for DNP3 and PakBus communications. The PakBus address of the DNP slave must match the DNP address of the slave.

** DNP over TCP **- Setting the ComPort parameter to a number >= 100 specifies a TCP port number and will enable listening on this TCP port for a socket connection instead of using one of the physical ComPorts. The default port number for DNP over TCP is 20000. The port number must match the port number specified in the DNP master for the master to make a connection with the datalogger.


# BaudRate (Data Transmit Rate)


The rate, in bps, at which data is transmitted. The options are 300, 1200, 2400, 4800, 9600, 19200, 38400, 57600, and 115200. Selecting one of these options fixes the baud rate at that rate of communication.

** NOTE:**This datalogger does not support autobaud mode.

** NOTE:**If a serial port is opened, it must be closed before changing the port baud rate.

Right-click this parameter to display a list.

** Type:**Constant. In [SerialOpen](serialopen.md), BaudRate can be a variable.


# Confirmation (DNP Confirmation)


A parameter that is used to determine whether DNP3 data link layer confirmation is enabled or disabled, and to set the timeout interval for data link layer and application layer confirmation. In general, the use of data link layer confirmation is not recommended. Data link layer confirmation is always disabled for a TCP/IP link. The datalogger will always use application layer confirmation when transmitting event data or multi-fragment responses. The parameter is entered in the form of XSSS. X = 0 enables data link layer confirmation. X = 1 disables data link layer confirmation. SSS is the number of seconds the datalogger should wait for a response to data link layer confirmation and/or application layer confirmation before timing out. A timeout > 0 should always be entered.

Examples:

- ** 1010 **: Disable link layer confirmation and set the application layer confirmation timeout to 10 seconds.

- ** 0006 **: Enable data link layer confirmation and set data link layer confirmation and application layer confirmation timeout to 6 seconds.

When using ComRS232, the datalogger usually goes into sleep mode after 40 seconds of inactivity on the communications port. After going to sleep with some interface methods it sometimes takes a packet of incoming data to wake it up and then a retry packet to get the message through. If packets continue arriving before the 40 second timeout, the datalogger should respond very quickly to the new packets.

Type: Constant or Variable


## Optional Parameters



# TimeOffset (Time Offset)


The local time offset, in seconds, from UTC. If the datalogger setting "UTC Offset" is a value other than -1 (disabled), this offset will be ignored and the UTC Offset value will be used. This parameter can be used to keep the datalogger clock set to local time when the DNP3 master is sending time synchronization commands to the datalogger, which are in UTC.

Type: Constant or Variable


# MaxTimeDiff (Maximimum Difference in Time)


The maximum difference in time (ms) between the datalogger clock and theDNP3

** Note:**Distributed Network Protocol is a set of communications protocols used between components in process automation systems. Its main use is in utilities such as electric and water companies.

master clock that will be tolerated before the clock is changed upon command from the DNP3 master. If a 0 is entered, the datalogger clock will be set upon receipt of a time synchronization command from the DNP3 master. If a -1 is entered, the datalogger clock will not be set. If the datalogger is connected to a DNP3 master and a Loggernet server, a -1 can be entered in order to avoid problems that can arise from a DNP3 master and Loggernet server both setting the datalogger clock.

Type: Constant or Variable


# DNPTLS


The DNPTLS parameter is an optional parameter used to enable TLS. If the parameter is set to 0 or is omitted, TLS is disabled. If the parameter is set to 1, TLS is enabled. Note that the CR800 Series, CR1000 and CR3000 do not support TLS.

Type: Constant or Variable
```
