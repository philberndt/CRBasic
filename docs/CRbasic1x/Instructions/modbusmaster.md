# ModbusMaster (Modbus Master)

The ModbusMaster instruction sets up a datalogger as aModbus

**Note:** Communication protocol published by Modicon in 1979 for use in programmable logic controllers (PLCs).

master device to send or retrieve data to/from a Modbus slave.

**NOTE:** ModbusMaster() is being replaced by ModbusClient(). ModbusMaster will be deprecated in 2023 and will no longer appear in CRBasic.Beginning with OS6.0, programs should be updated to use the [ModbusClient](modbusclient.md) instruction.

## Syntax

ModbusMaster(ResultCode,ComPort,BaudRate,ModbusAddr,Function,Variable,Start,Length,Tries,TimeOut,ModbusOption[optional] )

The following is an example program using the ModbusMaster instruction to set up a datalogger to read the holding registers of a Modbus slave device.

This program was written for aCR1000X, but other dataloggers can use similar code (voltage ranges, channel numbers, or other parameters may need to be changed to reflect the specifications of the datalogger).

```
'Declare Public Variables
Public PTemp, batt_volt,ModbusData(20),Result

'Define Data Tables
DataTable (Test,1,-1)
DataInterval (0,15,Sec,10)
Minimum (1,batt_volt,FP2,0,False)
Sample (1,PTemp,FP2)
Sample (1,ModbusData(),FP2)
EndTable

'Main Program
BeginProg
Scan (1,Sec,0,0)
PanelTemp (PTemp,15000)
Battery (Batt_volt)

'Retrieve Modbus Data
ModbusMaster(Result,COMRS232,115200,3,3,ModbusData(),1,10,3,100)

'Enter other measurement instructions
'Call Output Tables
CallTable Test
NextScan
EndProg
```

## Remarks

The datalogger supports Modbus functions 01-06, 15, and 16 (see following Function parameter). This instruction must be executed each time you request or fix data in the Modbus slave.

**NOTE:** If a Modbus device requires even parity, SerialOpen must be used in the program to set the parity to even (default is no parity).

If multiple ModbusMaster instructions are used in a program, and these instructions use the same ComPort, use the SemaphoreGet/SemaphoreRelease instructions to prevent the instructions from attempting to use the communications hardware at the same time.

## Parameters

# ResultCode (Result Code)

The ResultCode variable holds the results of the communication attempt. ResultCode is set to 0 if communication is successful; it increments by 1 after a communication failure (a failure is an expiration of the timeout period and any Tries). If the Modbus server returns a Modbus exception code, it will be preceded by a minus sign and stored in ResultCode. Error codes returned by the server:

| Code | Name                                                                                                                                                                                                        |
| ---- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| -01  | Illegal function: the function code received in the query is not an allowable action for the device. The device may not support the function or it may not be in a state to process the request.            |
| -02  | Illegal data address: the data address received in the query is not an allowable address for the device. The combination of the reference number and transfer length may be invalid.                        |
| -03  | Illegal data value: the value contained in the query data field is not an allowable value for the device.                                                                                                   |
| -04  | Server device failure: an unrecoverable error occurred while the device was attempting to perform the requested action.                                                                                     |
| -05  | Acknowledge: the device has accepted the request and is processing it, but a long duration of time will be required to do so. This is a specialized function used in conjunction with programming commands. |
| -06  | Server device busy: the device is engaged in processing a long-duration program command.                                                                                                                    |
| -08  | Memory parity error: the device attempted to read a record file but detected a parity error in the memory. Used in conjunction with function codes 20 and 21.                                               |
| -09  | Gateway path unavailable: indicates that the gateway was unable to allocate an internal communication path from the input port to the output port for processing the request.                               |
| -10  | ModbusClient error: received an unexpected function code response from server.                                                                                                                              |
| -11  | The specified ComPort (or TCP socket) is not opened. There is no connection to the Modbus server.                                                                                                           |
| -16  | ModbusClient error: out of comms memory.                                                                                                                                                                    |
| -20  | ModbusClient error: variable not dimensioned large enough to store results from server.                                                                                                                     |

Type: Variable

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

A variable can be used in the ComPort parameter for use with functions that return a communication port (for example,[TCPOpen](tcpopen.md)). If the parameter is not a COM port, then it is assumed to be a socket and the instruction will useTCP/IP

**Note:** Transmission Control Protocol / Internet Protocol.

Modbus protocol to communicate with a TCP/IP ModbusSlave device.

# BaudRate (Data Transmit Rate)

The rate, in bps, at which data is transmitted. The options are 300,600,1200, 2400, 4800, 9600, 19200, 38400, 57600, and 115200. Selecting one of these options fixes the baud rate at that rate of communication.If a negative baud rate is entered, the first communication attempt will be at the specified baud rate, but if communication fails at that rate, the datalogger will go into autobaud mode (where it will try different rates until successful or until the instruction times out).

**NOTE:** Autobaud is not available on control ports used as com ports. Baud rate for SDC ports must be 9600 or greater.If a serial port is opened, it must be closed before changing the port baud rate.

**NOTE:** If you are using SerialOpen to control a SDM-SIOx (SDM-SI01A, SDM-SIO1A, SDM-SIO2R) automatic baud rate detection is not supported. Rather, setting the baud rate to a negative value enables automatic flow control (RTS/CTS).Click herefor additional information.

Right-click this parameter to display a list.

**Type:** Constant. In [SerialOpen](serialopen.md), BaudRate can be a variable.

The datalogger uses 8 data bits and no parity for Modbus communication. This can be changed by preceding the Modbus instruction with a SerialOpen instruction that sets the desired data bit and parity values.

# ModbusAddr (Modbus Address)

The Modbus address of the server. Valid range is 1-255 (excluding 189, which is the PakBus sync byte). Address 0 is a broadcast address. Multiple addresses can be assigned to a single interface by using multiple ModbusClient() instructions

The ModbusAddr can be used to specify an offset for the starting register. Enter Starting Register \* 1000 + the address. (i.e., the most significant digits specify the offset and the last three digits specify the Modbus address). For instance, for a starting register of 1500 and an address of 3, enter 1500003.

**NOTE:** If ModbusAddr is set to a negative value, the datalogger will discard any bytes that are echoed back to it.

Type: Variable or Integer

For the ModbusMaster instruction, the ModbusAddr parameter is used to specify the address of the ModbusSlave with which you are trying to communicate. Valid ranges are 1 - 127 or 1 - 247 for Modbus/PakBus

**Note:** PakBus is a proprietary communication protocol developed by Campbell Scientific to facilitate communications between Campbell Scientific devices. Similar in concept to IP (Internet Protocol), PakBus is a packet-switched network protocol with routing capabilities. A registered trademark of Campbell Scientific, Inc.

.

# Function

Used to define what data is being sent to or received from the ModbusServer.

| Code | Name                       | Description                                                                                     |
| ---- | -------------------------- | ----------------------------------------------------------------------------------------------- |
| 01   | Read Coil/Port Status      | Reads the On/Off status of discrete output(s) in the ModbusServer                               |
| 02   | Read Input Status          | Reads the On/Off status of discrete input(s) in the ModbusServer                                |
| 03   | Read Holding Registers     | Reads the binary contents of holding register(s) in the ModbusServer                            |
| 04   | Read Input Registers       | Reads the binary contents of input register(s) in the ModbusServer                              |
| 05   | Force Single Coil/Por      | Forces a single Coil/Port in the ModbusServer to either On or Off                               |
| 06   | Write Single Register      | Writes a single register value (16-bit long) to a ModbusServer. (ModbusOption must be set to 1) |
| 15   | Force Multiple Coils/Ports | Forces multiple Coils/Ports in the ModbusServer to either On or Off                             |
| 16   | Write Multiple Registers   | Writes values into a series of holding registers in the ModbusServer                            |

Type: Variable or Constant

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

# Start

Specifies the 16 bit address of the first register that will be requested or acted upon. The Start parameter should be entered as a number 1 to 65535. The data address, or offset, in the Modbus frame sent to the server will be equal to "Start-1". For example, to access the extended holding register commonly referred to as 40001, use a Function code of 3 and a Start parameter of 1. Note, setting the Start parameter directly to a value of 40001 will not start access at the first holding register, it will start access 40000 registers into the holding register table; this is a common mistake when using ModbusClient.

Type: Variable of Constant

# Length

Specifies the number of CRBasic variables to act upon with this instruction. When reading coils or when using a 16-bit related ModbusOption parameter of 1, 3, 11, or 13, the Length parameter will match the number of 16 bit register values requested or acted upon. When using a 32-bit related ModbusOption parameter of 0,2,10, or 12, the datalogger will automatically double the Length value specified so that it requests 32-bits of data for each CRBasic variable.

Type: Variable of Constant

# Tries

The number of times the ModbusClient datalogger should attempt to communicate with the ModbusServer datalogger before moving on to the next instruction.

Type: Integer or Constant

# TimeOut (Time Out)

The amount of time, in 0.01 seconds, that the datalogger should wait for a response before considering the attempt a failure. If this parameter is negated, the timeout and any retries will take place in the background, and the datalogger will move on to the next instruction without waiting for this instruction to complete.

Type: Constant

## Optional Parameter

# ModbusOption (Modbus Option)

An optional parameter that is used to specify the data type of the Modbus variables.

| Code        | Description                                                                                                     |
| ----------- | --------------------------------------------------------------------------------------------------------------- |
| 0 or absent | Default; 32-bit float or Long, the standard ordering of the 2 byte registers are reversed (CDAB).               |
| 1           | If the Modbus variable array is defined as a Long, variables are treated as 16-bit signed integers.             |
| 2           | If the Modbus variable array is defined as a 32-bit float or a Long, with no reversal of the byte order (ABCD). |
| 3           | If the Modbus variable array is defined as a Long, variables are treated as 16-bit unsigned integers.           |
| 10          | Modbus ASCII, 4 bytes, CDAB.                                                                                    |
| 11          | Modbus ASCII, 2 bytes, signed.                                                                                  |
| 12          | Modbus ASCII, 4 bytes, ABCD.                                                                                    |
| 13          | Modbus ASCII, 2 bytes, unsigned.                                                                                |

Type: Variable

If a Modbus register address of greater than 40000 is needed, the ModbusAddr parameter for this instruction can take the form of rrrrrraaa, where aaa is the slave device address and the number rrrrrr is the register number offset required for the particular slave value. For example, if the slave register address is 401203 and the device address is 7, the parameter would be entered as 401000007 and the register number used would be 203 for the parameter ModbusStart.

To route Modbus packets over aPakBus

**Note:** PakBus is a proprietary communication protocol developed by Campbell Scientific to facilitate communications between Campbell Scientific devices. Similar in concept to IP (Internet Protocol), PakBus is a packet-switched network protocol with routing capabilities. A registered trademark of Campbell Scientific, Inc.

network, in the router s program, include a [DataGram()](datagram.md) instruction for every ModBus slave in the network (excluding the router). All slave PakBus addresses must match their ModbusSlave() addresses. If the Modbus Master is querying using TCP Modbus, then the DataGram() destination ID is 504 and source ID is 503. If the master is using RTU Modbus, then the DataGram() destination ID is 502 and the source ID is 501. At each of the remote slaves, the ModbusSlave() instruction s comport is -2 for TCP Modbus and 0 for RTU Modbus, which signals the Modbus Slaves that they are looking for Modbus via PakBus DataGrams carrying the Modbus request. If the DataGram destination ID is 506, then at the RTU end, Modbus TCP is converted to Modbus RTU before sending it out the comport. Conversely, when returning a response, it is converted back to Modbus TCP before sending it out on the PakBus network.
