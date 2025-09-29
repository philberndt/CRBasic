# ModbusSlave (Modbus Slave)

The ModbusSlave instruction configures a datalogger as a ModbusRTU

**Note:** Remote Telemetry Units

or ModbusTCP/IP

**Note:** Transmission Control Protocol / Internet Protocol.

slave device.

**NOTE:** ModbusSlave() is being replaced by ModbusServer(). ModbusSlave will be deprecated in 2023 and will no longer appear in CRBasic.Programs should be updated to use the [ModbusServer](modbusserver.md) instruction.

## Syntax

ModbusSlave(ComPort,BaudRate,ModbusAddr,ModbusVariable,BooleanVariable, [ModbusOption] )

Following are two examples of the ModbusSlave instruction.

Example 1, FP and Coils not declared (Coils map to C1..C8):

```
Public PTemp, batt_volt, ModResult
Public ModIn(80)

'Main Program
BeginProg
ModbusSlave(ComRS232,9600,1,ModIn(),0)

Scan (2,Sec,0,0)
PanelTemp (PTemp,4000)
Battery (Batt_volt)

ModIn(1) = batt_volt
ModIn(2) = PTemp
ModIn(3) = ModResult

NextScan
EndProg
```

Example 2, Integer, and coils defined as Boolean array:

```
Public PTemp, batt_volt, Counter
Public Port(6) as Boolean
Public ModIn(10) as long'Long provides integer conversion

'Main Program
BeginProg
ModbusSlave(ComRS232,9600,1,ModIn(),Port())

Scan (2,Sec,0,0)
PanelTemp (PTemp,4000)
Battery (Batt_volt)
Counter=Counter+1

ModIn(1) = batt_volt
ModIn(2) = PTemp
ModIn(3) = Counter

PortSet (1,Port(1))
PortSet (2,Port(2))

PortSet (SE1,Port(3))
PortSet (SE2,Port(4))
PortSet (SE3,Port(5))
PortSet (SE4,Port(6))
```

NextScan
EndProg

```

## Remarks


This instruction sets up a Modbus slave device. As a Modbus slave, the datalogger will respond to data request from a Modbus master over serial and TCP/IP. Supported Modbus functions are 01, 02, 03, 04, 05, 15, and 16. See the [ModbusMaster](modbusmaster.md) help for details on these functions.

**NOTE:** The datalogger uses 8 data bits and no parity for Modbus communication. This can be changed by preceding the Modbus instruction with a SerialOpen instruction that sets the desired data bit and parity values.

The instruction only needs to be executed once, and therefore may appear outside the main program scan. It is commonly inserted directly after the BeginProg statement. One or more instance of ModbusSlave() may be used in a single program. Multiple instances are most commonly used to configure multiple interfaces.

To route Modbus over a PakBus Network, in the router s program, include a DataGram() instruction for every Modbus Slave in the network. All slave PakBus addresses must match their ModbusSlave() addresses. If the Modbus Master is querying using TCP Modbus, then the DataGram() destination ID is 504 and source ID is 503. If the master is using RTU Modbus, then the DataGram() destination ID is 502 and the source ID is 501. At each of the remote slaves, the ModbusSlave() instructions comport is -2 for TCP Modbus and 0 for RTU Modbus, which signals the Modbus Slave that they are looking for Modbus via PakBus datagrams carrying the Modbus request.


## Parameters



# ComPort (Communications Port)


The COM port that will be used by the instruction. Right-click to display a list.

| Alphanumeric | Description |
| --- | --- |
| 0 | Modbus packets routed via PakBus (using Datagram protocol; application ID = 502) |
| ComUSB | USB port of the datalogger |
| ComRS232 | RS232 port of the datalogger |
| Com1 | Datalogger's control ports 1 (TX) & 2 (RX) |
| Com2 | COM2 port of the datalogger |
| Com3 | COM3 port of the datalogger |
| ComRF | Integrated RF communication |
| 502 - 65535 | Modbus TCP/IP, for data requests via Modbus TCP/IP protocol |

When communicating over TCP/IP, use the variable returned by the TCPOpen function for the ComPort.

Option 0, Modbus/PakBus, configures the datalogger to respond to Modbus RTU delivered via PakBus Datagrams with a destination Application ID of 502. A specific COM port is not specified as the Modbus request can be delivered over any active PakBus interface. This option is most commonly used in conjunction with an NL100 or with another logger configured with the DataGram() instruction. Additionally, this option is used when a user needs to route Modbus communications across a complex network or wants to use a single communications link for both Modbus and BMP5 communications via PakBus.

Setting the ComPort parameter to 502 or greater will specify the Modbus TCP/IP service port. The datalogger listens for Modbus TCP/IP communication on one IP port at a time; i.e., the logger will not listen on ports 502 and 503 at the same time. Avoid using port 6785 as that is the default PakBus TCP service port number.

A variable can be used in the ComPort parameter for use with functions that return a communication port.(for example,[TCPOpen](tcpopen.md)). If the ComPort parameter is set to 502, the datalogger will listen for a TCP connection on port number 502 and service TCP Modbus commands over TCP/IP instead of one one of the physical ComPorts.

Type: Constant


# BaudRate (Data Transmit Rate)


The rate, in bps, at which data is transmitted. The options are 300, 1200, 2400, 4800, 9600, 19200, 38400, 57600, and 115200. Selecting one of these options fixes the baud rate at that rate of communication.

**NOTE:** This datalogger does not support autobaud mode.

**NOTE:** If a serial port is opened, it must be closed before changing the port baud rate.

Right-click this parameter to display a list.

**Type:** Constant. In [SerialOpen](serialopen.md), BaudRate can be a variable.

This setting does not apply to Modbus TCP/IP or Modbus/Pakbus (ComPort = 0). A negative baud rate configures the port for automatic baud rate recognition (auto-baud). If set to auto-baud, the first communication attempt will be at the specified baud rate and failed communications will cause the datalogger to try different baud rates until successful or until the instruction times out.

The datalogger uses 8 data bits and no parity for Modbus communication. This can be changed by preceding the Modbus instruction with a SerialOpen instruction that sets the desired data bit and parity values.


# ModbusAddr (Modbus Address)


The Modbus address of the server. Valid range is 1-255 (excluding 189, which is the PakBus sync byte). Address 0 is a broadcast address. Multiple addresses can be assigned to a single interface by using multiple ModbusClient() instructions

The ModbusAddr can be used to specify an offset for the starting register. Enter Starting Register \* 1000 + the address. (i.e., the most significant digits specify the offset and the last three digits specify the Modbus address). For instance, for a starting register of 1500 and an address of 3, enter 1500003.

**NOTE:** If ModbusAddr is set to a negative value, the datalogger will discard any bytes that are echoed back to it.

Type: Variable or Integer

Multiple addresses can be assigned to a single interface by using multiple ModbusSlave() instructions

The ModbusAddr can be used to specify an offset for the starting register. Enter Starting Register \* 1000 + the address. (i.e., the most significant digits specify the offset and the last three digits specify the Modbus address). For instance, for a starting register of 1500 and an address of 3, enter 1500003.


# ModbusVariable (Modbus Variable)


Used to specify the variable array that is used as the source of data to send, or the variable array that is used as the destination for data received. This variable can be formatted for Coil, Floating Point, or Integer Modbus registers and can be formatted as either Floating Point or Integer. If Variable is declared as Long or Boolean, then this parameter is set as a Modbus integer; otherwise, floating point Modbus will be used. Longs are assumed to be signed integers (numbers will be in the range -32768..+32767). The datalogger does not differentiate between holding and input registers. The only difference is the address offset. The specified array will be used for requests of input (address offset of 30000) or holding (address offset of 40000) registers.

- If this parameter is declared asBoolean

  **Note:** Data type used to represent conditions or hardware that have only two states (true or false) such as flags and control ports.

  and a Coil function code of 1, 2, 5, or 15 is used, it will be mapped to a Modbus coil in the other device.

- If this parameter is declared asFloat

  **Note:** Four-byte floating-point data type. Default datalogger data type for Public or Dim variables. Same format as IEEE4.

  and a Register function code of 3, 4, or 16 is used, it will be mapped to a Modbus Holding or Input register as floating point.

- If this parameter is declared asLong

  **Note:** Data type used when declaring a variable as an integer.

  and a Register function code of 3, 4, or 16 is used, it will be mapped to Modbus Holding or Input registers as an integer. Longs are assumed to be signed integers (numbers will be in the range -32768..+32767).

If option 1 is used for ModbusOption, Variable should be declared as a long. If option 05, 06, 15, or 16 is used for Function, this parameter can be a constant.

Floating point variables take two Modbus registers. The Modbus input registers are offset by 30000; Modbus holding registers are offset by 40000. Therefore, the first register corresponding to any array location X is holding register 40000+2X-1. For example, to retrieve array value number 3, you would ask for two registers starting with 40005.

If the ModbusServer is queried by the client using an odd number of registers, then the ModbusServer datalogger assumes the request is for a 16 bit integer.

Type: Variable


# BooleanVariable (Boolean Variable)


A variable or variable array that is used to hold the result if the client sends one of the discrete on/off commands to the server (i.e., 01 Read Coil/Port Status, 02 Read Input Status, 05 Force Single Coil/Port, 15 Force Multiple Coils/Ports). This parameter must be dimensioned as aBoolean

**Note:** Data type used to represent conditions or hardware that have only two states (true or false) such as flags and control ports.

or a compiler error is returned.

If a 0 is entered for this parameter, then the discrete commands are mapped toC1, C2, SE1, SE2, SE3, and SE4instead. This will cause a compile warning, "A set coil function could conflict with a control port already in use." This is a warning, not an error, and the program can run successfully.

Type: Variable or Constant


## Optional Parameter



# ModbusOption (Modbus Option)


An optional parameter that is used to specify the data type of the Modbus variables.

| Code | Description |
| --- | --- |
| 0 or absent | Default; 32-bit float or Long, the standard ordering of the 2 byte registers are reversed (CDAB). |
| 1 | If the Modbus variable array is defined as a Long, variables are treated as 16-bit signed integers. |
| 2 | If the Modbus variable array is defined as a 32-bit float or a Long, with no reversal of the byte order (ABCD). |
| 3 | If the Modbus variable array is defined as a Long, variables are treated as 16-bit unsigned integers. |
| 10 | Modbus ASCII, 4 bytes, CDAB. |
| 11 | Modbus ASCII, 2 bytes, signed. |
| 12 | Modbus ASCII, 4 bytes, ABCD. |
| 13 | Modbus ASCII, 2 bytes, unsigned. |

Type: Variable

Notes:

- When communicating with other Modbus devices, the communications port, baud rate, data bits, stop bits, and parity are set in the Modbus driver for PC-based software or on the PLC.

- A datalogger acting as a Modbus slave returns a Modbus exception code 01 (Illegal Function) if a query is invalid, and an exception code of 02 (Illegal data address) if data values beyond the size of an array are requested. If a master requests variables that are not defined, the VarOutofBound field is incremented in the datalogger's Status table.

- The datalogger usually goes into sleep mode after 40 seconds of inactivity on the communications port. After going to sleep with some interface methods it sometimes takes a packet of incoming data to wake it up and then a retry packet to get the message through.

- Some Modbus devices (for example, some RTUs made by Bailey Controls that use less common CPUs) require reverse word order (MSW/LSW versus LSW/MSW) in the floating point format. Some software packages have a setting to work with this original Modbus format. For example, the Modicon 32-bit floating point order (0123 vs. 3201) advanced option must be enabled for the Modbus object in National Instruments Lookout.

See the [MoveBytes](movebytes.md) example for a program that converts an inverse floating point number to a floating point number.
```
