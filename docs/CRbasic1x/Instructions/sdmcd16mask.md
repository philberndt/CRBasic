# SDMCD16Mask (SDM-CD16AC Mask)

The SDMCD16Mask instruction is used to control an SDM-CD16AC relay control port device. Unlike the SDMCD16AC instruction, it allows for the ports to be activated via a mask. It is commonly used in conjunction with the TimedControl () instruction.

## Syntax

SDMCD16Mask(Source,SDMCD16Mask,SDMAddress)

The following program shows the use of the SDMCD16Mask instruction to control an SDM-CD16 device.

```
PipeLineMode

Const SECONDS_ON_SITE_1 = 5
Const SECONDS_ON_SITE_2 = 5
Const SECONDS_ON_SITE_3 = 5
Const SECONDS_ON_SITE_4 = 5

Const NUMBER_SITES = 4
Const CYCLE_TIME = SECONDS_ON_SITE_1+SECONDS_ON_SITE_2+SECONDS_ON_SITE_3+SECONDS_ON_SITE_4

Const SITE_1 = &h0001
Const SITE_2 = &h0002
Const SITE_3 = &h0004
Const SITE_4 = &h0008

Public sdmcd16ac_port(16) As Boolean'SDM-CD16AC port status.

'TimedControl () variables.
Dim set_valve(NUMBER_SITES,2) As Long
Dim index As Long
Dim mask_timedcontrol As Long

'Variables used in the manual control of SDM-CD16AC.
Dim set_port As Long
Dim mask_manual_control As Long
Dim i
Dim mask

TimedControl (NUMBER_SITES,CYCLE_TIME,Sec,&h0000,index,set_valve,1)

BeginProg
Set_valve(1,1) = SITE_1
Set_valve(2,1) = SITE_2
Set_valve(3,1) = SITE_3
Set_valve(4,1) = SITE_4

set_valve(1,2) = SECONDS_ON_SITE_1
set_valve(2,2) = SECONDS_ON_SITE_2
set_valve(3,2) = SECONDS_ON_SITE_3
set_valve(4,2) = SECONDS_ON_SITE_4

mask_timedcontrol = &h000F
mask_manual_control = &h000F0
Scan (1,Sec,10,0)

SDMCD16Mask(set_valve,mask_timedcontrol,0)'Timed control of ports 1 through 4.
'Indicate SDM-CD16AC port 1 through 4 status.
If ( index <> 0 ) Then
sdmcd16ac_port(1) = set_valve(index,1) AND SITE_1
sdmcd16ac_port(2) = set_valve(index,1) AND SITE_2
sdmcd16ac_port(3) = set_valve(index,1) AND SITE_3
sdmcd16ac_port(4) = set_valve(index,1) AND SITE_4
EndIf

'Set SDM-CD16AC ports 5 through 16 per user flags.
set_port = &h000
mask = &h0010
For i = 5 To 16
set_port = set_port OR (sdmcd16ac_port(i) AND mask)
mask = mask * 2
Next i
SDMCD16Mask(set_port,mask_manual_control,0)'CRBASIC program control of ports 5 through 16.
NextScan
EndProg
```

## Remarks

A port on an SDM-CD16AC is enabled/disabled (turned on or off) by sending an integer value to it using the SDMCD16Mask instruction. Each of the 16 least significant bits of the integer corresponds to a port on the SDM-CD16AC, where port 1 is bit 0 (the least significant bit). If a bit is set, the corresponding port is enabled.

## Parameters

# Source

A variable or variable array dimensioned as a Long that holds the integer value that will be sent to the SDM-CD16AC. The SDM-CD16AC ports will be enabled/disabled following the bit pattern of the Source variable. For example, if the decimal value 21845 (hex &h5555 or binary &b0101010101010101) is sent to the SDM-CD16AC using the SDMCD16Mask () instruction, all the odd ports will be enabled.

Type: Long

# SDMCD16Mask

Used to indicate which ports will change state. For example, if the mask was a decimal 15 (hex &h000F or binary &b0000000000001111) and the Source was a decimal value 21845 (hex &h5555 or binary &b0101010101010101), Ports 5 through 16 would remain unchanged, and ports 1 through 4, would be set according to the Source variable.

Type: Long

# SDMAddress (Address of Device)

Defines the address of the device with which to communicate. ValidSDM

**Note:** Synchronous Device for Measurement. A processor-based peripheral device or sensor that communicates with the datalogger via hardwire over a short distance using a protocol proprietary to Campbell Scientific.

addresses are 0 through 14. Address 15 is reserved for the SDMTrigger instruction.

Some SDM instructions support repetitions. If a Reps parameter is present and it is greater than 1, the data logger will increment the SDM address used in the instruction for each subsequent device with which it communicates.
