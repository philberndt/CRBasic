# SDMSIO4 (SDM-SIO4 Serial Input/Output Device)

The SDMSIO4 instruction is used to control and transmit/retrieve data from a Campbell Scientific SIO4 Interface (legacy 4 Channel Serial Input/Output device). See the SDM-SIO4 Serial Input Interface manual for operation details

**NOTE:** This instruction is not compatible with the newer SDM-SIO4A. The SDM-SIO4 is a legacy product that has been replaced with the SDM-SIO4A. The SerialOpen instruction is used to set up and control the SDM-SIO4A.

## Syntax

SDMSIO4(Dest,Reps,SDMAddress,Mode,Command,Param1,Param2,ValuesPerRep,Multiplier, Offset)

The following program sets up the SDM-SIO4 to accept serial data from a datalogger.

```
'---------------------- DECLARATIONS -------------------
Public SIO4Data(8)
Public DummyVariable
Public Flag(8)
'---------------------- OUTPUT TABLES -------------------

DataTable (SIO4,True,10000)
Sample (8,SIO4Data(),FP2)
EndTable
'---------------------- PROGRAM -----------------------

BeginProg
Scan (1,SEC,0,1)'One scan to set up SIO4 communication parameters
'Set SIO4 port4 communication parameters [COMMAND 2049]
SDMSIO4(DummyVariable,1,0,4,2049,3146,0000,0,1,0)
'3 = DTR and RTS always set, ignore CTS
'1 = 1 stop bit no parity
'4 = 8 data bits
'6 = 9600 baud
NextScan
'---------------------- MAIN SCAN -------------------

Scan (1000,MSEC,0,0)'non-burst,continous scan
If Flag(5) Then
'Activate SIO4 command line mode remotely [COMMAND 0007]
SDMSIO4(DummyVariable,1,0,1,0007,0000,0000,0,1,0)
Delay (1,10,MSEC)
Flag(5)=False
EndIf

'Set up receive filter on SIO4 port4 [COMMAND 2054]
SDMSIO4(DummyVariable,1,0,4,2054,9111,0000,0,1,0)
Delay(1,50,MSEC)

'Transmit filtered 10X data from SIO4 [COMMAND 0004]
SDMSIO4(SIO4Data(),1,0,4,0004,0000,0000,8,1,0)
Delay(1,20,MSEC)
'Flush SIO4 port4 receive buffer [COMMAND 0003]
SDMSIO4(DummyVariable,1,0,4,0003,0000,0000,0,1,0)
Delay(1,20,MSEC)
'Flush SIO4 converted data buffer [COMMAND 0009]
SDMSIO4(DummyVariable,1,0,4,0009,0000,0000,0,1,0)
Delay(1,10,MSEC)
CallTable SIO4
NextScan
EndProg'Program End
```

## Remarks

**NOTE:** Unlike other SDM instructions, the SDMSIO4 instruction is called from the datalogger's processing task. Therefore, if a delay is required between SDMSIO4 instructions, use the Delay instruction's Option 1 for parameter 1.

## Parameters

# Dest

The variable in which to store the results of the instruction when retrieving data from the SIO4. If data is being sent to the SIO4, then Dest becomes the source array for the data to be sent. The Dest array must be at least as large as the Reps parameter value multiplied by the ValuesPerRep parameter value.

Type: Variable or Array

# Reps

Defines the number of sequential SIO4s that will be called by the instruction. The datalogger will poll the SIO4 with the address set by the Address parameter first, receive or send the number of values set by the ValuesPerRep parameter next, and then poll the SIO4 with the next sequential address. If the Reps parameter is 2, the ValuesPerRep is 3, and the Command parameter is set to receive, then three values from the first SIO4 would be sent to the first three elements of the Dest array, and three values from the second SIO4 would be received and written to the fourth through sixth elements of the Dest array.

Constant integer (or expression that evaluates as a constant).

# SDMAddress (Address of Device)

Defines the address of the device with which to communicate. ValidSDM

**Note:** Synchronous Device for Measurement. A processor-based peripheral device or sensor that communicates with the datalogger via hardwire over a short distance using a protocol proprietary to Campbell Scientific.

addresses are 0 through 14. Address 15 is reserved for the SDMTrigger instruction.

Some SDM instructions support repetitions. If a Reps parameter is present and it is greater than 1, the data logger will increment the SDM address used in the instruction for each subsequent device with which it communicates.

# SIO4Mode

Defines which port the instruction will affect.

| Code | Description                |
| ---- | -------------------------- |
| 1    | Send/Receive Port 1        |
| 2    | Send/Receive Port 2        |
| 3    | Send/Receive Port 3        |
| 4    | Send/Receive Port 4        |
| 5    | Send to all Ports (global) |

# SIO4Cmd

Used to configure the SIO4. The commands are listed briefly below. See the SDM-SIO4 manual for details.

| Code | Description                                                                              |
| ---- | ---------------------------------------------------------------------------------------- |
| 1    | Poll of available data                                                                   |
| 2    | Get EPROM and memory signatures.                                                         |
| 3    | Flush all receive buffers.                                                               |
| 4    | Send data to datalogger.                                                                 |
| 5    | Return number of watchdog errors, invalid command executed, and lithium battery voltage. |
| 6    | Flush transmit buffer.                                                                   |
| 7    | Activate command line.                                                                   |
| 8    | Poll TX buffers for data.                                                                |
| 9    | Flush converted data buffer.                                                             |
| 66   | Send single-byte data to datalogger.                                                     |
| 67   | Get return code                                                                          |
| 320  | Send byte data to SDM-SIO4.                                                              |
| 321  | Execute command line command.                                                            |
| 1024 | Send string to SIO4.                                                                     |
| 1025 | Transmit a byte.                                                                         |
| 1026 | Serial port status.                                                                      |
| 1027 | Manual handshake mode.                                                                   |
| 2049 | Communication parameters.                                                                |
| 2054 | Set up receive filter.                                                                   |
| 2304 | Transmit string and/or data to device (formatter/filter).                                |
| 2305 | Transmit bytes.                                                                          |

# Param1

The first parameter that should be passed to the SIO4 for the selected Command. Refer to the SDM-SIO4 manual for details.

# Param2

The second parameter that should be passed to the SIO4 for the selected Command. Refer to the SDM-SIO4 manual for details.

# ValuesPerRep

The number of values to be sent or received from each SIO4 each time this instruction is performed.

# Mult, Offset (Multiplier and Offset)

Factors by which to scale the raw results of the measurement. Typically used to convert the raw measurement to engineering units or to units other than which is output. For example, the TCDiff instruction measures a thermocouple and outputs temperature in degrees C. A multiplier of 1.8 and an offset of 32 will convert the temperature to degrees F.

For temperature measurements, a multiplier (mult) of 1 and an offset of 0, would output in degrees Celsius. For analog measurements, a multiplier (mult) of 1 and an offset of 0, would output the measured voltage in millivolts divided by the excitation voltage in volts.

If Repetitions of greater than 1 are used for this instruction, Repetitions can also be used for the Multiplier and Offset.[SeeMultipliers, Offsets, and Disable Variables with Repetitionsfor more information.](../Info/multipliersoffsets.md)

Type: Constant, Variable, Array, or Expression
