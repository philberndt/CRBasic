# TGA (TGA100A, TGA200, or TGA200A)

The TGA instruction is used to measure a TGA100A, TGA200, or TGA200A trace gas analyzer system. It can also be used to measure a TGA100 with an upgraded CPU module.

## Syntax

TGA(Dest,SDMAddress,DataList,ScanMode)

Following is an example program for measuring the TGA trace gas analyzer.

```
'Declare Public Variables
Public RefTemp, batt_volt
Public TGAData(31)'destination array for TGA data

Alias TGAData(1) = ConcA
Alias TGAData(2) = ConcB
Alias TGAData(3) = ConcC
Alias TGAData(4) = TGAStatus
Alias TGAData(5) = TGApressure
Alias TGAData(6) = LaserTemp
Alias TGAData(7) = DCCurrentA
Alias TGAData(8) = DCCurrentB
Alias TGAData(9) = DCCurrentC
Alias TGAData(10) = TGAAnalog1
Alias TGAData(11) = TGATemp1
Alias TGAData(12) = TGATemp2
Alias TGAData(13) = LaserHeater
Alias TGAData(14) = RefDetSignalA
Alias TGAData(15) = RefDetSignalB
Alias TGAData(16) = RefDetSignalC
Alias TGAData(17) = RefDetTransA
Alias TGAData(18) = RefDetTransB
Alias TGAData(19) = RefDetTransC
Alias TGAData(20) = RefDetTemp
Alias TGAData(21) = RefDetCooler
Alias TGAData(22) = RefDetGainOffset
Alias TGAData(23) = SmpDetSignalA
Alias TGAData(24) = SmpDetSignalB
Alias TGAData(25) = SmpDetSignalC
Alias TGAData(26) = SmpDetTransA
Alias TGAData(27) = SmpDetTransB
Alias TGAData(28) = SmpDetTransC
Alias TGAData(29) = SmpDetTemp
Alias TGAData(30) = SmpDetCooler
Alias TGAData(31) = SmpDetGainOffset

'Declare Constants for TGA
const TGA_SDMAddress = 0
const TGA_DataList = 4
const TGA_ScanMode = 3

'Define Data Tables
DataTable (Test,1,-1)
DataInterval (0,15,Sec,10)
Minimum (1,batt_volt,FP2,0,False)
Sample (1,RefTemp,FP2)
Sample (31,TGAData,IEEE4)'store all TGA data
EndTable

'Main Program
BeginProg
Scan (1,Sec,0,0)
PanelTemp (RefTemp,15000)
Battery (Batt_volt)
'Enter other measurement instructions
TGA(TGAData, TGA_SDMAddress, TGA_DataList, TGA_ScanMode)'get TGA data
'Call Output Tables
CallTable Test
NextScan
EndProg
```

## Parameters

# Dest

The array in which to store the results of the measurement. The length of the input variable array depends on the values of parameters DataList and ScanMode.

Type: Variable Array

# SDMAddress (Address of Device)

Defines the address of the device with which to communicate. ValidSDM

**Note:** Synchronous Device for Measurement. A processor-based peripheral device or sensor that communicates with the datalogger via hardwire over a short distance using a protocol proprietary to Campbell Scientific.

addresses are 0 through 14. Address 15 is reserved for the SDMTrigger instruction.

Some SDM instructions support repetitions. If a Reps parameter is present and it is greater than 1, the data logger will increment the SDM address used in the instruction for each subsequent device with which it communicates.

# DataList

Specifies the data to be retrieved from the sensor. If DataList = 1, only concentration and status are returned. If DataList = 2, then laser temperature, pressure, and DC Current are returned in addition to concentration and status. If DataList = 3, then the TGA analog signal 1 and TGA temperatures are also returned. If DataList = 4, then all data except DutyCycle1 and DutyCycle2 are returned. If DataList = 5, then all data are returned. See the list below for complete information:

| Name             | Description                                                                                                        | DataList | ScanMode |
| ---------------- | ------------------------------------------------------------------------------------------------------------------ | -------- | -------- |
| ConcA            | Trace gas concentration measured in ramp A (ppmv)                                                                  | 1        | 1        |
| ConcB            | Trace gas concentration measured in ramp B (ppmv)                                                                  | 1        | 2        |
| ConcC            | Trace gas concentration measured in ramp C (ppmv)                                                                  | 1        | 3        |
| TGAStatus        | Status flags: see table below                                                                                      | 1        | 1        |
| TGAPressure      | Pressure in the analyzer vacuum manifold (mb)                                                                      | 2        | 1        |
| LaserTemp        | emperature of the laser mounting plate (K)                                                                         | 2        | 1        |
| DCCurrentA       | Laser DC current for ramp A (mA)                                                                                   | 2        | 1        |
| DCCurrentB       | Laser DC current for ramp B (mA)                                                                                   | 2        | 2        |
| DCCurrentC       | Laser DC current for ramp C (mA)                                                                                   | 2        | 3        |
| TGAAnalog1       | TGA Analog Channel 1 voltage (V)                                                                                   | 3        | 1        |
| TGATemp1         | Temperature of TGA detector mounting plate ( C)                                                                    | 3        | 1        |
| TGATemp2         | Temperature of TGA dewar mounting plate ( C)>                                                                      | 3        | 1        |
| LaserHeater      | Voltage applied to the heater on the laser mounting plate to maintain the laser at the specified temperature (V)   | 4        | 1        |
| RefDetSignalA    | Reference detector signal at the center of the spectral scan for ramp A (mV)                                       | 4        | 1        |
| RefDetSignalB    | Reference detector signal at the center of the spectral scan for ramp B (mV)                                       | 4        | 2        |
| RefDetSignalC    | Reference detector signal at the center of the spectral scan for ramp C (mV)                                       | 4        | 3        |
| RefDetTransA     | Reference detector transmittance at the center of the spectral scan for ramp A (%)                                 | 4        | 1        |
| RefDetTransB     | Reference detector transmittance at the center of the spectral scan for ramp B (%)                                 | 4        | 2        |
| RefDetTransC     | Reference detector transmittance at the center of the spectral scan for ramp C (%)                                 | 4        | 3        |
| RefDetTemp       | Reference detector temperature ( C)                                                                                | 4        | 1        |
| RefDetCoole      | Current applied to the thermoelectric cooler to maintain the reference detector at its specified temperature (arb) | 4        | 1        |
| RetDetGainOffset | Gain and offset settings for the reference detector preamplifier (arb)                                             | 4        | 1        |
| mpDetSignalA     | Sample detector signal at the center of the spectral scan for ramp A (mV)                                          | 4        | 1        |
| SmpDetSignalB    | Sample detector signal at the center of the spectral scan for ramp B (mV)                                          | 4        | 2        |
| SmpDetSignalC    | Sample detector signal at the center of the spectral scan for ramp C (mV)                                          | 4        | 3        |
| SmpDetTransA     | Sample detector transmittance at the center of the spectral scan for ramp A (%)                                    | 4        | 1        |
| SmpDetTransB     | Sample detector transmittance at the center of the spectral scan for ramp B (%)                                    | 4        | 2        |
| SmpDetTransC     | Sample detector transmittance at the center of the spectral scan for ramp C (%)                                    | 4        | 3        |
| SmpDetTemp       | Sample detector temperature ( C)                                                                                   | 4        | 1        |
| SmpDetCooler     | Current applied to the thermoelectric cooler to maintain the sample detector at its specified temperature (arb)    | 4        | 1        |
| SmpDetGainOffset | Gain and offset settings for the sample detector preamplifier (arb)                                                | 4        | 1        |
| DutyCycle1       | Duty cycle for the TGA heater 1                                                                                    | 5        | 1        |
| DutyCycle2       | Duty cycle for the TGA heaster 2                                                                                   | 5        | 1        |

Status Flags - The TGAStatus value gives an indication of the overall status of the TGA. A value of zero indicates a normal condition. A nonzero value indicates one or more of the bits are set. The meaning of each of the bits is given below.

| Bit | Decimal Value | Description                                                   |
| --- | ------------- | ------------------------------------------------------------- |
| 0   | 1             | Line Lock for ramp A is OFF                                   |
| 1   | 2             | Line Lock for ramp B is OFF                                   |
| 2   | 4             | Line Lock for ramp C is OFF                                   |
| 3   | 8             | Sample detector signal exceeded input range                   |
| 4   | 16            | Reference detector signal exceeded input range                |
| 5   | 32            | Sample detector temperature is outside its specified range    |
| 6   | 64            | Reference detector temperature is outside its specified range |
| 7   | 128           | Laser temperature outside its specified range                 |
| 8   | 256           | Pressure is above is upper limit                              |

Type: Constant

# ScanMode

Specifies the number of values to be retrieved for scan-specific data. Normally the ScanMode parameter corresponds to the TGA100A "number of ramps" parameter that specifies how many absorption lines are being measured. If ScanMode is set to a lower number than the TGA100A "number of ramps" parameter, the data for ramp B and/or C will not be retrieved from the TGA100A. If ScanMode is set to a higher number than the TGA100A "number of ramps" parameter, the TGA100A will return zero for the ramp B and/or C values.

Type: Constant
