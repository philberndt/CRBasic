# SDMAO4A (Set SDM-AO4A Voltages)

This instruction is used to configure an SDM-AO4A four channel analog output device which is used for proportional control or driving strip charts.

## Syntax

SDMAO4(Source,SDMAO4ADest,SDMAddress,SDMAO4AStartChan,Reps,SDMAO4AOption)

The following program is for aCR1000Xdatalogger. Some parameter values may need to be changed for other dataloggers (for instance, range values in measurement instructions).

The example program is a for weather station with a datalogger measuring wind speed, wind direction, temperature, and relative humidity. Each parameter is then scaled to 0 to 1000 mVDC, and output to a strip chart recorder through the SDM-AO4A.

```
'Declare Variables
Public WS_ms
Public WD_0_360
Public Temp_C
Public RH
Public WD_0_540
Public AO4AOutput(4)
Public AO4AResponse
Alias AO4AOutput(1) = WSOut
Alias AO4AOutput(2) = WDOut
Alias AO4AOutput(3) = TempOut
Alias AO4AOutput(4) = RHOut

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
'Send mV outputs to SDM-AO4A at SDM Address 12 (Rotary Switch at C)
SDMAO4A(AO4AOutput(), AO4AResponse,12,1,4,1)
NextScan
EndProg
```

## Remarks

The SDM-AO4A includes four independent, continuous, analog outputs (CAO), which are used for proportional control or driving strip charts. Measured or processed values in the datalogger are scaled to millivolts and transferred to the SDMAO4A as digital values. The SDM-AO4A then performs a digital to analog conversion and outputs an analog voltage signal.

The SDMAO4A will respond to the older SDMAO4 instruction, but you will not be able to take advantage of any of the SDMAO4A's additional features. If the older instruction is used, the device will use the default option code 1.

## Parameters

# Source

The variable or variable array that holds the voltage(s), in millivolts, that will be sent to the SDM-AO4A(s). If multiple SDM-AO4As are to be triggered with one instruction, this parameter must be dimensioned to the total number of channels for all the devices being set (e.g., if all four channels are being set on two SDM-AO4 devices, Source must be dimensioned to eight).

Type: Variable or variable array

# SDMAO4ADest

A variable that holds a status code indicating success or failure of the instruction.

| Response Code | Description                          |
| ------------- | ------------------------------------ |
| 240           | Successful                           |
| 241           | Signature error                      |
| 242           | Current overload error               |
| 243           | Current overload and signature error |

A current overload error occurs when current overload protection is triggered (130 mA, +/- 15 mA). A signature error usually indicates noise on the line.

Any other response code returned indicates failed communication.

Type: Variable

# SDMAddress (Address of Device)

Defines the address of the device with which to communicate. ValidSDM

**Note:** Synchronous Device for Measurement. A processor-based peripheral device or sensor that communicates with the datalogger via hardwire over a short distance using a protocol proprietary to Campbell Scientific.

addresses are 0 through 14. Address 15 is reserved for the SDMTrigger instruction.

Some SDM instructions support repetitions. If a Reps parameter is present and it is greater than 1, the data logger will increment the SDM address used in the instruction for each subsequent device with which it communicates.

# SDMAO4AStartChan

Used to define the first channel on the SDMAO4A that should be set. Any reps will occur on subsequent channels.

Type: Constant

# Reps

Determines the number of SDM-AO4A output channels that will be set. If this parameter is greater than four (i.e., voltage is being set for more than one SDM-AO4 device), voltage is set on the next consecutively addressed SDM-AO4A device. In this case, the SDM-AO4As must have sequential SDM addresses.

Type: Constant integer (or expression that evaluates as a constant).

# SDMAO4AOption

Used to set the operating mode for the SDMAO4A.

| Option Code | Description     |
| ----------- | --------------- |
| 0           | Power down      |
| 1           | 5V synchronous  |
| 2           | 5V sequential   |
| 3           | 10V synchronous |
| 4           | 10V sequential  |

In the synchronous mode, all channels are set at the same time. This mode is slower since for large changes in voltage it may take multiple charging cycles to arrive at the final voltage. The steps occur at 5 ms intervals, thus, for a 10V step it may take up to three charge cycles (or 15 ms) to settle to the 16-bit level.

In sequential mode, the channels are set sequentially. The output signal can take from 600 usecs to 1 ms (worst case) to settle to 16-bit resolution with a 10V step change. The four outputs then update 1 ms apart.

Type: Constant
