# SDMCD16AC (SDM-CD16AC Relay Control Port Device)

The SDMCD16AC instruction is used to control an SDM-CD16AC relay control port device.

**NOTE:** This instruction is used for both the SDM-CD16ACA and the SDM-CD16AC.

## Syntax

SDMCD16AC(Source,Reps,SDMAddress)

### Example #1

```
'This example demonstrates how to control individual channels on one SDM.

'Wiring
'Data logger SDM-CD16AC
'C1 (SDM-C1 on CR3000)--C1
'C2 (SDM-C2 on CR3000)--C2
'C3 (SDM-C3 on CR3000)--C3
'12V--------------------12V
'Ground-----------------Ground

'\\\\\\\\\\\\\\\\\\\\\\\\\ DECLARATIONS /////////////////////////
'Declare Public Variable
```

Public CD16_1(16)' Set the SDM address to 1 on this CD16AC
Alias CD16_1(1) = Solenoid_1
Alias CD16_1(2) = Spare_2
Alias CD16_1(3) = Spare_3
Alias CD16_1(4) = Spare_4
Alias CD16_1(5) = Spare_5
Alias CD16_1(6) = Spare_6
Alias CD16_1(7) = Spare_7
Alias CD16_1(8) = Spare_8
Alias CD16_1(9) = Spare_9
Alias CD16_1(10) = Spare_10
Alias CD16_1(11) = Spare_11
Alias CD16_1(12) = Spare_12
Alias CD16_1(13) = Spare_13
Alias CD16_1(14) = Spare_14
Alias CD16_1(15) = Spare_15
Alias CD16_1(16) = Spare_16

'\\\\\\\\\\\\\\\\\\\\\\\\\\\ PROGRAM ////////////////////////////
'Main Program
BeginProg
Scan (5,Sec,0,0)
If TimeIntoInterval (0,15,Sec) Then
Solenoid_1 = 1'Non zero value is On
Else
Solenoid_1 = 0'Zero is Off
EndIf

```
'Add additional conditions for each additional channel used
```

SDMCD16AC(CD16_1,1,1)
NextScan
EndProg

### Example #2

```
'This example demonstrates how to control individual channels on 2 SDMs.
'Wiring (Note: When multiple SDMs are used, all of them connect to the same
'C1, C2, C3 terminals on the data logger.)
'Data logger SDM-CD16AC
'C1 (SDM-C1 on CR3000)--C1
'C2 (SDM-C2 on CR3000)--C2
'C3 (SDM-C3 on CR3000)--C3
'12V--------------12V
'Ground----------Ground

'If using more than one SDM,SDM addresses must be in consecutive order.
'In this example, SDM addresses 1 and 2 are used.

'\\\\\\\\\\\\\\\\\\\\\\\\\ DECLARATIONS /////////////////////////
'Declare Public Variables
'Declare control array; for multiple SDMs array size is number of SDMs*16
'CD16(1-16) are used for SDM 1 channels 1-16; CD16(17-32) are used for SDM 2
'channels 1-16.
```

Public CD16(32)
Public WaterLevel,Temp

```
'variables to hold measurement outputs; this program assumes that
'water level and temperature measurements have been made and that the results are stored
'in the appropriate variables. 13 spare channels are available on each SDM to add control
'for additional channels.
```

'\\\\\\\\\\\\\\\\\\\\\\\\\ SDM-1 /////////////////////////
Alias CD16(1) = Pump_1'1st SDM controls pumps
Alias CD16(2) = Pump_2
Alias CD16(3) = Pump_3
Alias CD16(4) = Spare_4
Alias CD16(5) = Spare_5
Alias CD16(6) = Spare_6
Alias CD16(7) = Spare_7
Alias CD16(8) = Spare_8
Alias CD16(9) = Spare_9
Alias CD16(10) = Spare_10
Alias CD16(11) = Spare_11
Alias CD16(12) = Spare_12
Alias CD16(13) = Spare_13
Alias CD16(14) = Spare_14
Alias CD16(15) = Spare_15
Alias CD16(16) = Spare_16

'\\\\\\\\\\\\\\\\\\\\\\\\\ SDM-2 /////////////////////////
Alias CD16(17) = Heater_1'second SDM controls heaters
Alias CD16(18) = Heater_2
Alias CD16(19) = Heater_3
Alias CD16(20) = Spare2_4
Alias CD16(21) = Spare2_5
Alias CD16(22) = Spare2_6
Alias CD16(23) = Spare2_7
Alias CD16(24) = Spare2_8
Alias CD16(25) = Spare2_9
Alias CD16(26) = Spare2_10
Alias CD16(27) = Spare2_11
Alias CD16(28) = Spare2_12
Alias CD16(29) = Spare2_13
Alias CD16(30) = Spare2_14
Alias CD16(31) = Spare2_15
Alias CD16(32) = Spare2_16

'\\\\\\\\\\\\\\\\\\\\\\\\\\\ PROGRAM ////////////////////////////
'Main Program
BeginProg
Scan (5,Sec,0,0)

```
'Add measurements here; for this example these would be temperature and water level
'measurements.
```

'\\\\\\\\\\\\\\\\\\\\\\\\\ SDM-1 Channel Control /////////////////////////
If WaterLevel > 3 Then 'controls SDM-1 Channel 1
Pump_1 = 1 'Non zero value is On
Else
Pump_1 = 0 'Zero is Off
EndIf
If WaterLevel > 5 Then 'controls SDM-1 Channel 2
Pump_2 = 1 'Non zero value is On
Else
Pump_2 = 0 'Zero is Off
EndIf
If WaterLevel > 10 Then 'controls SDM-1 Channel 3
Pump_3 = 1 'Non zero value is On
Else
Pump_3 = 0 'Zero is Off
EndIf

```
'Add additional logic control for channels 4-16 on SDM-1

'\\\\\\\\\\\\\\\\\\\\\\\\\\\ SDM-2 Channel Control /////////////////////////////
```

If Temp < 35 Then 'controls SDM-2 Channel 1
Heater_1 = 1 'Non zero value is On
Else
Heater_1 = 0 'Zero is Off
EndIf
If Temp < 30 Then 'controls SDM-2 Channel 2
Heater_2 = 1 'Non zero value is On
Else
Heater_2 = 0 'Zero is Off
EndIf
If Temp < 20 Then
Heater_3 = 1 'controls SDM-2 Channel 3
Else
Heater_3 = 0 'Zero is Off
EndIf

```
'Add additional logic control for channels 4-16 on SDM-2

'send control variables to the two SDM-CD16ACs
```

SDMCD16AC(CD16(),2,1)'reps = 2 for 2 SDMs
NextScan
EndProg

## Remarks

A port on an SDM-CD16AC is enabled/disabled (turned on or off) by sending a value to it using the SDMCD16AC instruction. A non-zero value will enable the port; a zero value disables it. The values to be sent to the SDM-CD16AC are held in the Source array.

## Parameters

# Source

An array (dimensioned as Float, Long, or Boolean) which holds the values that will be sent to the SDM-CD16AC to enable/disable its ports. An SDM-CD16AC has 16 ports; therefore, in most instances the source array should be dimensioned to 16 times the number of Repetitions (the number of SDM-CD16AC devices to be controlled). As an example, with the array CDCtrl(32), the value held in CDCtrl(1) will be sent to port 1, the value held in CDCtrl(2) will be sent to port 2, etc. The value held in CDCtrl(32) would be sent to port 16 on the second SDM-CD16AC.

If the Source parameter is defined as a Long variable, but it is dimensioned less than 16 \* Reps, Source will act as a binary control for the instruction whose bits 0..15 will specify control ports 1..16, respectively. In this instance, Source(1) will be used for the first rep, Source(2) will be used for the second, and so on.

Type: Variable defined as Long, Float, or Boolean

# Reps

The number of SDM-CD16AC devices that will be controlled with this instruction.

Type: Constant integer (or expression that evaluates as a constant).

# SDMAddress (Address of Device)

Defines the address of the device with which to communicate. ValidSDM

**Note:** Synchronous Device for Measurement. A processor-based peripheral device or sensor that communicates with the datalogger via hardwire over a short distance using a protocol proprietary to Campbell Scientific.

addresses are 0 through 14. Address 15 is reserved for the SDMTrigger instruction.

Some SDM instructions support repetitions. If a Reps parameter is present and it is greater than 1, the data logger will increment the SDM address used in the instruction for each subsequent device with which it communicates.
