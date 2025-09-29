# CDM_VW300Dynamic (Measure and Diagnose VWIRE 305 or CDM-VW30x)

The CDM_VW300Dynamic instruction is used to obtain measurements and diagnostic information from a VWIRE 305 or CDM-VW300 or CDM-VW305 at a sub-second interval. For additional information on controller area network peripherals and Campbell modules, see [CPI and CDM](https://www.campbellsci.com/news-cpi-cdm).

## Syntax

CDM_VW300Dynamic(CPIAddress,DestFreq,DestDiag)

The following code can be used for initializing the variables for the parameters in the CDM_VW300Config instruction for a VW305. If a VW300 is programmed, the device type would be set to 0 and all arrays for the device would be set to 2 elements rather than 8.

```
Public DeviceType As Long = 1
```

Public CPIaddress As Long = 1

```
Public SysOptions As Long = 0
```

| `'`                            | `CH1`      | `CH2`    | `CH3`    | `CH4`    | `CH5`    | `CH6`    | `CH7`    | `CH8`     |
| ------------------------------ | ---------- | -------- | -------- | -------- | -------- | -------- | -------- | --------- |
| `Dim ChanEnable(8) As Long =`  | `{ 1,`     | `1,`     | `1,`     | `1,`     | `1,`     | `1,`     | `1,`     | `1 }`     |
| `Dim ResonAmp(8) =`            | `{ 0.002,` | `0.002,` | `0.002,` | `0.002,` | `0.002,` | `0.002,` | `0.002,` | `0.002 }` |
| `Dim LowFreq(8) =`             | `{ 300,`   | `300,`   | `300,`   | `300,`   | `300,`   | `300,`   | `300,`   | `300 }`   |
| `Dim HighFreq(8) =`            | `{ 6000,`  | `6000,`  | `6000,`  | `6000,`  | `6000,`  | `6000,`  | `6000,`  | `6000 }`  |
| `Dim ChanOptions(8) As Long =` | `{ 0,`     | `0,`     | `0,`     | `0,`     | `0,`     | `0,`     | `0,`     | `0 }`     |
| `Dim Mult(8) =`                | `{ 1.0,`   | `1.0,`   | `1.0,`   | `1.0,`   | `1.0,`   | `1.0,`   | `1.0,`   | `1.0 }`   |
| `Dim Offset(8) =`              | `{ 0.0,`   | `0.0,`   | `0.0,`   | `0.0,`   | `0.0,`   | `0.0,`   | `0.0,`   | `0.0, }`  |
| `Dim SteinA(8) =`              | `{ 0.0,`   | `0.0,`   | `0.0,`   | `0.0,`   | `0.0,`   | `0.0,`   | `0.0,`   | `0.0, }`  |
| `Dim SteinB(8) =`              | `{ 0.0,`   | `0.0,`   | `0.0,`   | `0.0,`   | `0.0,`   | `0.0,`   | `0.0,`   | `0.0, }`  |
| `Dim SteinC(8) =`              | `{ 0.0,`   | `0.0,`   | `0.0,`   | `0.0,`   | `0.0,`   | `0.0,`   | `0.0,`   | `0.0, }`  |
| `Dim RF_MeanBins(8) As Long =` | `{ 20,`    | `20,`    | `20,`    | `20,`    | `20,`    | `20,`    | `20,`    | `20, }`   |
| `Dim RF_AmpBins(8) As Long =`  | `{ 20,`    | `20,`    | `20,`    | `20,`    | `20,`    | `20,`    | `20,`    | `20, }`   |
| `Dim RF_LowLim(8) =`           | `{ 300,`   | `300,`   | `300,`   | `300,`   | `300,`   | `300,`   | `300,`   | `300, }`  |
| `Dim RF_HighLim(8) =`          | `{ 6000,`  | `6000,`  | `6000,`  | `6000,`  | `6000,`  | `6000,`  | `6000,`  | `6000 }`  |
| `Dim RF_Hyst(8) =`             | `{ 1.0,`   | `1.0,`   | `1.0,`   | `1.0,`   | `1.0,`   | `1.0,`   | `1.0,`   | `1.0, }`  |
| `Dim RF_Form(8) As Long =`     | `{ 111,`   | `111,`   | `111,`   | `111,`   | `111,`   | `111,`   | `111,`   | `111, }`  |

The followingCR1000Xcode programs a VWIRE 305 or CDM-VW300 to make 20 Hz dynamic measurements.

```
'Program to read 20Hz data from one VWIRE 305 or CDM-VW300 device (2 channels)
'Make sure the CPI address given here
'matches what is shown in Device Configuration Utility or DVW Toolbox
'for the intended VWIRE 305 or CDM-VW300 series device
Const CPI_ADDR = 5

Public Freq(2)'dynamic frequencies
Public Diag(2) As Long'diagnostic code

Public StaticFreq(2)'Static (1Hz output) frequencies
Public Therm(2)'Thermistor readings
'Standard Deviation of the dynamic
'readings that occurred during the latest
'1 second interval
Public DynStdDev(2)

'The following arrays are used to configure the
'VWIRE 305 or CDM-VW300 series device. Refer to the CDM_VW300Config
'instruction used below
' CH1 CH2
' --- ---
'Set to true (Enabled=1, Disabled=0) only those channels which have sensors connected
Dim Enable(2) As Long = { 1, 1}
'Specify the target/desired resonant amplitude at which the sensor will be maintained
'via excitation, given in Volts. This should be in the range 0.010 to 0.001
Dim Max_AMP(2) = { 0.002, 0.002}
'Low Frequency Boundary (sensor frequency should never fall below
'this value regardless of environmental changes)
Dim F_Low(2) = { 300, 300}
'High Frequency Boundary (sensor frequency should never exceed
'this value regardless of environmental changes)
Dim F_High(2) = { 6000, 6000}
'Output Format - Hz vs. Hz^2 :: Value of 0 measured frequency is given in units of Hz,
'Value of 1 measured frequency is squared and given in units of Hz^2
Dim OutForm(2) As Long = { 0, 0}
'Multiplier (factor) to be applied to sensor output frequency
Dim Mult(2) = { 1.0, 1.0}
'Offset (shift) to be applied to sensor output frequency
Dim Off(2) = { 0.0, 0.0}
'Steinhart-Hart coefficients [A,B,C] for converting thermistor ohms to
'temperature in Celsius. Specifying zeroes for A,B,C results in a reading in Ohms.
Dim SteinA(2) = { 0.0, 0.0}
Dim SteinB(2) = { 0.0, 0.0}
Dim SteinC(2) = { 0.0, 0.0}

'Rainflow configuration (not used in this program,
'but required as configuration arguments)
Dim RFMB(2) As Long = { 20, 20}
Dim RFAB(2) As Long = { 20, 20}
Dim RFLL(2) = { 400.0, 400.0}
Dim RFHL(2) = {4000.0,4000.0}
Dim RFHY(2) = { 0.005, 0.005}
Dim RFOF(2) As Long = { 100, 100}

'Configure the VWIRE or CDM-VW300 series device
'Use the variable arrays declared above
CDM_VW300Config(0,CPI_ADDR,0,Enable(),Max_AMP(),F_Low(),F_High(), _
OutForm(),Mult(),Off(), SteinA(),SteinB(),SteinC(), _
RFMB(),RFAB(),RFLL(),RFHL(),RFHY(),RFOF())

DataTable (static,true,-1)
'Static Frequency reading (1Hz output)
Sample (2,StaticFreq(),IEEE4)
'Thermistor reading : Ohms or DegC
Sample (2,Therm(),IEEE4)
'Standard Deviation of dynamic readings
'taken during the most recent second
Sample (2,DynStdDev(),IEEE4)
EndTable

DataTable (dynamic,true,-1)
'Dynamic Frequency (20Hz output)
Sample (2,Freq(),IEEE4)
'Diagnostic code for the current dynamic reading
Sample (2,Diag(),IEEE4)
EndTable

BeginProg

'20 Hz/50msec scan rate
Scan(50,msec,500,0)
CDM_VW300Dynamic(CPI_ADDR,Freq(),Diag())'Get dynamic readings
CallTable dynamic

If TimeIntoInterval (0,1,Sec) Then' Process static data only once per second
CDM_VW300Static(CPI_ADDR,StaticFreq(),Therm(),DynStdDev())'Get static readings
CallTable static
EndIf

NextScan
EndProg
```

## Remarks

The CDM_VW300Dynamic instruction measures the resonant frequency or square of the resonant frequency of a vibrating wire sensor, depending upon the ChanOptions of the CDM_VW300Config instruction. The update/output interval of the measurement is based on the scan rate at which the instruction is executed. The CDM_VW300Config instruction's multipliers and offsets are applied to the results of the measurements before being stored in the DestFreq variable. This instruction should be run in the main scan of the CRBasic program.

This instruction also stores diagnostic information for the measurements. Refer to [CDM-VW300 Diagnostic Codes](cdmvw300dynamicdiagnosticcodes1.md) for additional information.

## Parameters

# CPIAddress (CPI Address)

TheCPI

**Note:** CPI is a proprietary interface for communications between Campbell Scientific dataloggers and Campbell Scientific CDM peripheral devices. It consists of a physical layer definition and a data protocol. CDM devices are similar to Campbell Scientific SDM devices in concept, but the use of the CPI bus enables higher data-throughput rates and use of longer cables. CDM devices require more power to operate in general than do SDM devices.

address configured in theCDM **Note:** module. Valid range is 1 through 120. Note that the CPIAddress cannot be a variable because the datalogger must know all information for a given CDM module at program compile.(e.g., before the program runs).

Type: Constant integer

** NOTE:**UseDevice Configuration Utility

** Note:**Configuration tool used to set up dataloggers and peripherals, and to configure PakBus settings before those devices are deployed in the field and/or added to networks.

, DVW Toolbox software, or the CPIAddModule instruction to configure the CPIAddress within your VWIRE 305 or CDM-VW300 series device.

# DestFreq (Measurement Values Destination)

The variable array in which to store the measurement values streaming from the device. The values are output in Hz or Hz^2, depending upon the CDM_VW300Config instruction. The array should be dimensioned to two for the CDM-VW300 or eight for the VWIRE 305 or CDM-VW305.

Type: Variable array

# DestDiag (Destination for Diagnostic Values)

The variable array in which to store diagnostic values. The array should be dimensioned to two for the CDM-VW300 or eight for the VWIRE 305 or CDM-VW305.

Type: Variable array
