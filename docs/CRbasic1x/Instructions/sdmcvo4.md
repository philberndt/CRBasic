# SDMCVO4 (SDM-CVO4 Current/Voltage Output Device)

This instruction is used to control the SDM-CVO4 four channel current/voltage output device.

## Syntax

SDMCVO4(CVO4Source,CVO4Reps,SDMAddress,CVO4Mode)

This program example is for a weather station measuring wind speed, wind direction, temperature, and relative humidity. Each parameter is scaled to 0 to 10000 mVDC, and output to a SCADA system through the SDM-CVO4.

```
'Declare Variables
Public WS_ms
Public WD_0_360
Public Temp_C
Public RH
Public WD_0_540
Public CVO4Output(4)
Alias CVO4Output(1) = WSOut
Alias CVO4Output(2) = WDOut
Alias CVO4Output(3) = TempOut
Alias CVO4Output(4) = RHOut

'Code for DataTable OneMin
DataTable(OneMin,1,-1)
DataInterval (0,1,Min,0)
WindVector (1, WS_ms,WD_0_360, IEEE4, 0, 0, 0, 0)
Average (1,Temp_C,IEEE4,0)
Sample (1,RH, IEEE4)
EndTable

BeginProg
Scan (1,Sec,1,0)
'Code for 03001 wind measurments, WS_ms & WD_0_360:
PulseCount(WS_ms, 1,P1,1, 1, 0.75, 0.2)
BRHalf(WD_0_360, 1,mv5000C,1,Vx1, 1, 1000, True, 1000,15000, 355, 0)

'Code for CS500 measurement, AirTC and RH:
VoltSe(Temp_C,1,mv5000C,3,0, 0,15000,0.1,-40.0)

VoltSe(RH,1,mv5000C,2,0, 0,15000,0.1,-40.0)

'Call Data Table
CallTable (OneMin)
'Convert 0-360 WD to 0-540:
If WD_0_540 >= 270 AND WD_0_360 <180 Then
WD_0_540 = WD_0_360 + 360
Else
WD_0_540 = WD_0_360
EndIf
'Scale the measurements for the SDM-CVO4 to output 0-10000 mV
WSOut = WS_ms*200'WS: 0-50 m/s = 0-10000 mV
WDOut = WD_0_540 *18.59'WD: 0-540 deg = 0-10000mV
TempOut = 100*(Temp_C+40)'Temp: -40-60 deg C = 0-10000 mV
RHOut = RH *100'RH: 0-100 % RH = 0-10000 mV
'Send mV outputs to SDM-CVO4 using the option to override the switch settings
SDMCVO4(CVO4Output(),4,0,10)
NextScan
EndProg
```

## Remarks

This instruction controls the SDM-CVO4, which outputs a voltage or a current. Internal jumpers are used to set the mode for the device, but the jumpers can be overridden with the Mode parameter in this instruction.

## Parameters

# CVO4Source

A variable array that holds the values for the voltages (millivolts) or currents (microamps) that will be output by each channel of the device (Source(1) sets channel1, Source(2) sets channel2, etc.). When outputting a voltage, the variable must be within the range of 0 to 10,000. When outputting a current, the variable must be within the range of 0 to 20,000.

Type: Variable array

# CVO4Reps

Indicates the number of channels to set to the defined voltage or current. Additional SDM-CVO4 devices can be controlled by one SDMCVO4 instruction by assigning them consecutive addresses and setting the CVO4Reps parameter to a value equal to the total number of channels of all devices (e.g., to set all four channels on two devices, set the CVO4Reps parameter to 8).

If the CVO4Reps parameter is set to 0, power to the device will be turned off.

Type: Constant (or expression that evaluates as a constant)

# SDMAddress (Address of Device)

Defines the address of the device with which to communicate. ValidSDM

**Note:** Synchronous Device for Measurement. A processor-based peripheral device or sensor that communicates with the datalogger via hardwire over a short distance using a protocol proprietary to Campbell Scientific.

addresses are 0 through 14. Address 15 is reserved for the SDMTrigger instruction.

Some SDM instructions support repetitions. If a Reps parameter is present and it is greater than 1, the data logger will increment the SDM address used in the instruction for each subsequent device with which it communicates.

# CVO4Mode

Determines what type of signal will be output by the device. Right-click the parameter to display a list box of valid options.

| Option | Description                                      |
| ------ | ------------------------------------------------ |
| 0      | Voltage output, use jumper settings (scale only) |
| 1      | Current output; use jumper settings (scale only) |
| 10     | Voltage output; override jumper setting          |
| 11     | Current output; override jumper setting          |

The two override options affect all of the channels of all of the SDM-CVO4 devices being controlled by this instruction. These two options override the hardware settings in the device. Use of this mode takes approximately 2 milliseconds per device. When either of these options is used you lose the flexibility of setting the output for each channel individually.

Type: Variable
