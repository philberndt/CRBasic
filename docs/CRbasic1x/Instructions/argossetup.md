# ArgosSetup (Set Up Argos)

The ArgosSetup instruction is used to set up the datalogger for transmitting data via an Argos satellite.

## Syntax

ArgosSetup(ResultCode,ST20Buffer,DecimalID,HexadecimalID,Frequency)

This example shows the use of the ArgosSetup instruction. Note: In this example, the DecimalID and HexadecimalID are invalid.

```
Public SetupResult
Const Frequency = 401680000
Const DecimalID=00000
Const HexadecimalID=&H00000

BeginProg
Scan (60,Sec,3,0)
if not SetupResult then
ArgosSetup(SetupResult,0,DecimalID,HexadecimalID,Frequency)
endif
NextScan
EndProg
```

## Parameters

# ResultCode (Instruction Results)

A variable that holds the result of the instruction. The instruction stores a -1 (True) if the transmission is successful or 0 (False) if it fails. If 2 is returned, the transmitter is not connected to the datalogger's communications port or there is some hardware problem with the connection.

Type: Variable declared as a Float, Boolean, or long

# ST20Buffer (ST20 Buffers)

The number of the ST20 buffer that should be set up. Valid entries are 0 through 6 (ArgosData, ArgosTransmit) or 0 through 7 (ArgosSetup).

Type: Constant Integer

# DecimalID (Decimal ID Number)

The decimal ID number assigned to the buffer.

Type: Constant Integer

# HexadecimalID (Hexadecimal ID Number)

The hexadecimal ID number assigned to the buffer.

Type: Constant Integer

# Frequency

The frequency (Hz) to be assigned to the ST20Buffer. Valid entries are 401630000 to 401656000 in steps of 2000 Hz and 401676000, 401678000, and 401680000.

Type: Constant Integer

**NOTE:** The assigned DecimalID and HexadecimalID are different values (e.g., the DecimalID is not merely a decimal representation of the HexadecimalID).
