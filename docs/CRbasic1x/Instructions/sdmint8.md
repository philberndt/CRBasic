# SDMINT8 (SDM-INT8 Interval Timer Module)

The SDMINT8 instruction is used to program and control the SDM INT8 interval timer module.

## Syntax

SDMINT8(Dest,SDMAddress,Config8_5,Config4_1,Funct8_5,Funct4_1,OutputOpt,CaptureTrig,Mult, Offset)

This example demonstrates using the SDMINT8 interval timer instruction to measure 5 wind sensors attached to the device. Two other wind sensors are measured on pulse channels 1 and 2. Code is included to set the measurements for the INT8 to 0 if they are equal to or less than the offset for the SDMINT8 measurement instruction.

```
'Declare Variables
Public Int8(5), Wind_ms(2)
Dim I

DataTable (IntDat,True,-1)
DataInterval (0,5,Min,10)
Average (1,Wind_ms(),FP2,False)
Average (5,Int8(),FP2,False)
EndTable

BeginProg
Scan (5,Sec,3,0)
PulseCount (Wind_ms(),2,P1,1,1,1.75,.2)
'Measure channels 1 - 5 on INT8
SDMInt8(Int8(),0,0002,2222,0002,2222,0,1,1.75,.2)
'If INT8 measurement is less than offset, set to 0
For i = 1 to 5
If Int8(i)<0.21 then Int8(i)=0
Next i
CallTable (IntDat)
NextScan
EndProg
```

## Remarks

The INT8 is a device for the measurement of intervals, counts between events, frequencies, periods, and/or time since an event using the SDM communications protocol. Refer to the INT8 manual for more information about its capabilities.

## Parameters

# Dest

The variable or array where the results of the instruction are stored. For all output options except Capture All Events, the Dest argument should be a one-dimensional array with as many arguments as there are programmed INT8 channels. If the Capture All Events output option is selected, then the Dest array must be two dimensional. The magnitude of the first dimension should be set to the number of functions (up to 8), and the magnitude of the second dimension should be set to at least the maximum number of events to be captured. The values will be loaded into the array in the sequence of all of the time ordered events captured from the lowest programmed channel to those of the highest programmed channel.

Type: Variable Array

# SDMAddress (Address of Device)

Defines the address of the device with which to communicate. ValidSDM

**Note:** Synchronous Device for Measurement. A processor-based peripheral device or sensor that communicates with the datalogger via hardwire over a short distance using a protocol proprietary to Campbell Scientific.

addresses are 0 through 14. Address 15 is reserved for the SDMTrigger instruction.

Some SDM instructions support repetitions. If a Reps parameter is present and it is greater than 1, the data logger will increment the SDM address used in the instruction for each subsequent device with which it communicates.

# Config8_5

A four-digit code used to configure channels 5 through 8 on the INT8. Each input channel can be configured for either high or low level voltage inputs and for rising or falling edges. The digits represent the channels in descending order from left to right (e.g., 8 7 6 5).

| Code | Description              |
| ---- | ------------------------ |
| 0    | High level, rising edge  |
| 1    | High level, falling edge |
| 2    | Low level, rising edge   |
| 3    | Low level falling edge   |

Type: Constant

# Config4_1

A four-digit code used to configure channels 1 through 4 on the INT8. Each input channel can be configured for either high or low level voltage inputs and for rising or falling edges. The digits represent the channels in descending order from left to right (e.g., 4 3 2 1).

| Code | Description              |
| ---- | ------------------------ |
| 0    | High level, rising edge  |
| 1    | High level, falling edge |
| 2    | Low level, rising edge   |
| 3    | Low level falling edge   |

Type: Constant

# Funct8_5

A four digit code used to program the timing function of channels 5 through 8. Similar to the Config parameters, digits represent the channels in descending order from left to right (e.g., 8 7 6 5).

| Code | Description                                                                             |
| ---- | --------------------------------------------------------------------------------------- |
| 0    | No value returned                                                                       |
| 1    | Period (ms) between edges on the programmed channel                                     |
| 2    | Frequency (kHz) of edges on the programmed channel                                      |
| 3    | Time (ms) between an edge of the previous channel and an edge of the programmed channel |
| 4    | Time (ms) between an edge on Channel 1 and edge on the programmed channel               |
| 5    | Number of edges on channel 2 since last edge on channel 1 using linear interpolation    |
| 6    | Low resolution frequency (kHz) of edges on programmed channel                           |
| 7    | Total count of edges on programmed channel since last interrogation                     |
| 8    | Number of edges on channel 2 since last edge on channel 1 without linear interpolation  |

Type: Constant

# Funct4_1

A four digit code used to program the timing function of channels 1 through 4. Similar to the Config parameters, digits represent the channels in descending order from left to right (e.g., 4 3 2 1).

| Code | Description                                                                             |
| ---- | --------------------------------------------------------------------------------------- |
| 0    | No value returned                                                                       |
| 1    | Period (ms) between edges on the programmed channel                                     |
| 2    | Frequency (kHz) of edges on the programmed channel                                      |
| 3    | Time (ms) between an edge of the previous channel and an edge of the programmed channel |
| 4    | Time (ms) between an edge on Channel 1 and edge on the programmed channel               |
| 5    | Number of edges on channel 2 since last edge on channel 1 using linear interpolation    |
| 6    | Low resolution frequency (kHz) of edges on programmed channel                           |
| 7    | Total count of edges on programmed channel since last interrogation                     |
| 8    | Number of edges on channel 2 since last edge on channel 1 without linear interpolation  |

Type: Constant

# OutputOpt

A numeric code that is used to select one of the five different output options. The selected option will be applied to all of the INT8 channels. A brief explanation is given below for each code. See the INT8 manual for detailed explanations of each option. Right click the parameter to display a drop-down list box.

|       |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| ----- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---- | ----------- | --- | --- | --- | --- | --- | ------- | --- | --- | ------------------- | --- | ---------------- | ----------------------------------------------------------- | --- | ---------------- | ---------------------------------------------------------- | --- |
| 0     | Stores an average of the event data since the last time that the INT8 was interrogated by the datalogger. If no edges were detected, 0 will be returned for frequency and count functions, and 99999 will be returned for the other functions. The INT8 ceases to capture events during communications with the datalogger, thus some edges may be lost.                                                                                                                                                                                                                                |
| 32768 | Performs continuous averaging, which is utilized when input frequencies have a slower period than the execution interval of the datalogger. If an edge was not detected for a channel since the last time that the INT8 was polled, then the datalogger will not update the input location for that channel. The INT8 will capture events even during communications with the datalogger.                                                                                                                                                                                               |
| nnnn  | Averages the input values over "nnnn" milliseconds. The datalogger program is delayed by this instruction while the INT8 captures and processes the edges for the specified time duration and sends the results back to the datalogger. If no edges were detected, 0 will be returned for frequency and count functions, and 99999 will be returned for the other functions                                                                                                                                                                                                             |
| -nnnn | Instructs the INT8 to capture all events until "nnnn" edges have occurred on channel 1, until the datalogger addresses the INT8 with the CaptureTrig argument true, or until 8000 events have been captured. When the CaptureTrig argument is true, the INT8 will return up to the last nnnn events for each of the programmed INT8 channels, reset its memory, and begin capturing the next nnnn events. The INT8 waits for the first edge on channel 1 as a trigger to start making measurements. The Dest parameter must be dimensioned large enough to receive the captured events. |
| -9999 | Initiates a self memory test of the INT8. A numeric code is returned to indicate the results of the test.                                                                                                                                                                                                                                                                                                                                                                                                                                                                               | Code | Description |     | --- | --- |     | 0   | Bad ROM |     | -0  | Bad ROM and bad RAM |     | positive integer | Good ROM (value returned is the ROM signature) and good RAM |     | negative integer | Good ROM (value returned is the ROM signature) and bad RAM |     |

Type: Constant

# CaptureTrig

This argument is used when the Capture All Events output option is used. When CaptureTrig is true, the INT8 will return the last nnnn events.

Type: Variable

# Mult, Offset (Multiplier and Offset)

Factors by which to scale the raw results of the measurement. Typically used to convert the raw measurement to engineering units or to units other than which is output. For example, the TCDiff instruction measures a thermocouple and outputs temperature in degrees C. A multiplier of 1.8 and an offset of 32 will convert the temperature to degrees F.

For temperature measurements, a multiplier (mult) of 1 and an offset of 0, would output in degrees Celsius. For analog measurements, a multiplier (mult) of 1 and an offset of 0, would output the measured voltage in millivolts divided by the excitation voltage in volts.

If Repetitions of greater than 1 are used for this instruction, Repetitions can also be used for the Multiplier and Offset.[SeeMultipliers, Offsets, and Disable Variables with Repetitionsfor more information.](../Info/multipliersoffsets.md)

Type: Constant, Variable, Array, or Expression
