# SDMGeneric (Generic SDM Device)

The SDMGeneric instruction is used to send commands to an SDM device that is otherwise unsupported in the datalogger.

## Syntax

SDMGeneric(Dest,SDMAddress,CmdByte,NumValuesOut,Source,NumValuesIn,BytesPerValue,BigEndian,DelayByte)

This example program shows the use of the SDMGeneric instruction to check for the OS Version and OS signature check for the SDM-SIO1A.

```
'Simple OS Version and OS sig check for the SDM-SIO1A
'Variables and constants for the version number and signature checking
Public Ver_Value As String * 25'Holds version as text string
Public Sig_Value As String * 4'Holds SIG of OS as four byte HEX string
Public Sig_Value_Dec'Holds sig as a decimal number
'Change this address to match the SDM-SIO1A SDM Adress
Const SDM_Address=0
Dim SRC As String *1
Const cmd = 5'constant cmd = 0..7
Const bytes_out = 1'constant number of bytes out
Const bytes_val = 1'constant bytes per value (1,2,4)
Const big_endian = 1'constant 1 (big endian) or 0 (little endian)
Const Delay_usec = -0'constant delay between outgoing bytes (negative means delay 'also for incoming bytes
Const Ver_values_in = 20'constant number of values to receive
Const Sig_values_in = 4'constant number of values to recieve

SequentialMode

BeginProg
SDMSPeed (30)'Fix the speed
Ver_Value = ""
Sig_Value = ""
Scan (1,Sec,0,0)
'Use the generic SDM instruction to get extra info from the SDM-SIO1A
'Ask for the operating system version
Src = CHR(1)
SDMGeneric(Ver_Value,SDM_Address,cmd,bytes_out,Src,Ver_values_in,bytes_val,big_endian,delay_usec)

'Read signature
Src = CHR(2)

SDMGeneric(Sig_Value,SDM_Address,cmd,bytes_out,Src,Sig_values_in,bytes_val,big_endian,delay_usec)
Sig_Value_Dec = HexToDec (Sig_Value)'Convert sig to decimal too.
NextScan
EndProg
```

## Parameters

# Dest

A variable that will hold any incoming bytes from the SDM device.

Type: Variable or variable array

# SDMAddress (Address of Device)

Defines the address of the device with which to communicate. ValidSDM

**Note:** Synchronous Device for Measurement. A processor-based peripheral device or sensor that communicates with the datalogger via hardwire over a short distance using a protocol proprietary to Campbell Scientific.

addresses are 0 through 14. Address 15 is reserved for the SDMTrigger instruction.

Some SDM instructions support repetitions. If a Reps parameter is present and it is greater than 1, the data logger will increment the SDM address used in the instruction for each subsequent device with which it communicates.

# CmdByte

A setup byte that will be sent to the SDM device.

Type: Variable or constant

# NumValsOut

The number of values that are to be sent to the SDM device.

Type: Variable or constant

# Source

A variable that holds the values to be sent to the SDM device.

Type: Variable

# NumValsIn

The number of values that are expected to be received back from the SDM device.

Type: Variable or constant

# BytesPerValue

The number of bytes for each value sent to or received from the SDM device. Typically, this is 1, 2, or 4, for 1 byte, 2 byte, or 4 byte values.

Type: Variable or constant

# BigEndian

Indicates the order of bytes to be sent or received. Enter 0 for little endian (least significant byte first), or 1 for big endian (most significant byte first).

Type: Constant

# DelayByte

The delay, in microseconds, that should be used between sending bytes. If a negative value is entered, this is the delay to be expected between receiving bytes from the SDM device.

Type: Constant
