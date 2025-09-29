# SDMAO4 (Set SDM-AO4 Voltages)

This instruction is used to set the voltage for an SDM-AO4 four channel analog output device.

## Syntax

SDMAO4(Source,Reps,SDMAddress)

This program example is for a weather station measuring wind speed, wind direction, temperature, and relative humidity. Each parameter is then scaled to 0 to 1000 mVDC, and output to a strip chart recorder through the SDM-AO4.

```
'Declare Variables
Public WS_ms
Public WD_0_360
Public Temp_C
Public RH
Public WD_0_540
Public AO4Output(4)

Alias AO4Output(1) = WSOut
Alias AO4Output(2) = WDOut
Alias AO4Output(3) = TempOut
Alias AO4Output(4) = RHOut

'Code for DataTable OneMin
DataTable(OneMin,1,-1)
DataInterval(0,1,Min,0)
WindVector(1, WS_ms,WD_0_360, IEEE4, 0, 0, 0, 0)
Average(1,Temp_C,IEEE4,0)
Sample(1,RH, IEEE4)
EndTable

BeginProg
Scan(1,Sec,1,0)
'Code for 03001 wind measurments, WS_ms & WD_0_360:
PulseCount(WS_ms, 1,P1,1, 1, 0.75, 0.2)
BRHalf(WD_0_360, 1,mv5000C,1,Vx1, 1, 1000, True, 1000,15000, 355, 0)

'Code for CS500 measurement, AirTC and RH:
VoltSe(Temp_C,1,mv5000C,3,0, 0,15000,0.1,-40.0)

VoltSe(RH,1,mv5000C,2,0, 0,15000,0.1,-40.0)

'Call Data Table
CallTable(OneMin)
'Convert 0-360 WD to 0-540:
If WD_0_540 >= 270 AND WD_0_360 <180 Then
WD_0_540 = WD_0_360 + 360
Else
WD_0_540 = WD_0_360
EndIf
'Scale the measurements for the SDM-AO4 to output 0-1000 mV
WSOut = WS_ms*20'WS: 0-50 m/s = 0-1000 mV
WDOut = WD_0_540 *1.852'WD: 0-540 deg = 0-1000mV
TempOut = 10*(Temp_C+40)'Temp: -40-60 deg C = 0-1000 mV
RHOut = RH *10'RH: 0-100 % RH = 0-1000 mV
'Send mV outputs to SDM-AO4
SDMAO4(AO4Output(),4,12)
NextScan
EndProg
```

## Parameters

# Source

The variable or variable array that holds the voltage(s), in millivolts, that will be sent to the SDM-AO4(s). If multiple SDM-AO4s are to be triggered with one instruction, this parameter must be dimensioned to the total number of channels for all the devices being set (e.g., if all four channels are being set on two SDM-AO4 devices, Source must be dimensioned to eight).

Type: Variable of variable array

# Reps

Determines the number of SDM-AO4 output channels that will be set. If this parameter is greater than four (i.e., voltage is being set for more than one SDM-AO4 device), voltage is set on the next consecutively addressed SDM-AO4 device. In this case, the SDM-AO4s must have sequential SDM addresses.

# SDMAddress (Address of Device)

Defines the address of the device with which to communicate. ValidSDM

**Note:** Synchronous Device for Measurement. A processor-based peripheral device or sensor that communicates with the datalogger via hardwire over a short distance using a protocol proprietary to Campbell Scientific.

addresses are 0 through 14. Address 15 is reserved for the SDMTrigger instruction.

Some SDM instructions support repetitions. If a Reps parameter is present and it is greater than 1, the data logger will increment the SDM address used in the instruction for each subsequent device with which it communicates.
