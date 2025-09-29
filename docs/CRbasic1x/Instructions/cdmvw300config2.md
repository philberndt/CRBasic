# CDM_VW300Config (Configure VWIRE 305 or CDM-VW30x)

The CDM_VW300Config instruction is used to set up a VWIRE 305, CDM-VW300 or CDMVW305 for vibrating wire measurements. For additional information on controller area network peripherals and Campbell modules, see [CPI and CDM](https://www.campbellsci.com/news-cpi-cdm).

## Syntax

CDM_VW300Config(DeviceType,CPIAddress,SysOptions,ChanEnable,ResonAmp,LowFreq, HighFreq,ChanOptions,Mult,Offset,SteinA, SteinB, SteinC,RF_MeanBins,RF_AmpBins,RF_LowLim,RF_HighLim,RF_Hyst,RF_Form)

The following code can be used for initializing the variables for the parameters in the CDM_VW300Config instruction for a VW305. If a VW300 is programmed, the device type would be set to 0 and all arrays for the device would be set to 2 elements rather than 8.

```
Public DeviceType As Long = 1
Public CPIaddress As Long = 1
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
'VWIRE or CDM-VW300 series device. Refer to the CDM_VW300Config
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

'Configure the VWIRE 305 or CDM-VW300 series device
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

The CDM_VW300Config instruction should be placed before the BeginProg statement in the CRBasic program. Before starting the datalogger program, the datalogger configures the specified CDM device according to the settings given as parameters of this instruction.

## Parameters

# DeviceType (Device Type)

The type of module being configured. Right-click in the parameter box to display a drop-down list of defined variables.

Type: Constant

# CPIAddress (CPI Address)

TheCPI

**Note:** CPI is a proprietary interface for communications between Campbell Scientific dataloggers and Campbell Scientific CDM peripheral devices. It consists of a physical layer definition and a data protocol. CDM devices are similar to Campbell Scientific SDM devices in concept, but the use of the CPI bus enables higher data-throughput rates and use of longer cables. CDM devices require more power to operate in general than do SDM devices.

address configured in theCDM **Note:** module. Valid range is 1 through 120. Note that the CPIAddress cannot be a variable because the datalogger must know all information for a given CDM module at program compile.(e.g., before the program runs).

Type: Constant integer

** NOTE:**This instruction cannot be used to set the CPI address stored within a VWIRE 305, CDM-VW300 or CDM-VW305 device. That must be done usingDevice Configuration Utility

** Note:**Configuration tool used to set up dataloggers and peripherals, and to configure PakBus settings before those devices are deployed in the field and/or added to networks.

, DVW Toolbox software, or the [CPIAddModule](cpiaddmodule2.md) instruction.

# SysOptions (System Options)

Determines if a numeric value orNAN

** Note:**Not a number. A data word indicating a measurement or processing error. Voltage overrange, SDI-12 sensor error, and undefined mathematical results can produce NAN.

will be stored when a flag condition occurs, and whether or not the diagnostic LEDs are on or off. The following are valid options:

| Code | Description                                                                |
| ---- | -------------------------------------------------------------------------- |
| 0    | Display numeric output when diagnostic flags are set; LEDs are operational |
| 1    | Display NANs when diagnostic flags are set; LEDs are operational           |
| 10   | Display numeric output when diagnostic flags are set; LEDs are disabled    |
| 11   | Display NANs when diagnostic flags are set; LEDs are disabled              |

Disabling the LEDs offers some measure of power conservation.

Type: Variable

Refer to the [CDM_VW300Dynamic](cdmvw300dynamic2.md) instruction for additional information on diagnostic flags.

# ChanEnable (Enable or Disable Channels)

A variable that enables or disables the channels on a VWIRE 305 or CDM-VW3XX device. This parameter is dimensioned to two for the CDM-VW300 and eight for the VWIRE 305 or CDM-VW305. For each element of the array, enter a ** 0**or a ** 1**to disable or enable the corresponding channels.

Type: Variable

# ResonAmp (Resonant Amperage)

Holds the desired output voltage amplitude of the steady-state resonant response of the sensor in units of Volts. This parameter is dimensioned to two for the CDM-VW300 and eight for the VWIRE 305 or CDM-VW305. For each element of the array, enter the voltage for the corresponding channel number. The default value is 0.002V.

Type: Variable

# LowFreq, HighFreq (Lower and Upper Bounds of Resonant Frequency)

Sets the lower and upper bounds of the resonant frequency expected on the sensor. The parameters are dimensioned to two for the CDM-VW300 and eight for the VWIRE 305 or CDM-VW305. Valid ranges depend upon the scan rate, according to the following:

| Sample Rate | Min Frequency | Max Frequency |
| ----------- | ------------- | ------------- |
| 20 Hz       | 290 Hz        | 6000 Hz       |
| 50 Hz       | 290 Hz        | 6000 Hz       |
| 100 Hz      | 580 Hz        | 6000 Hz       |
| 200 Hz      | 1150 Hz       | 6000 Hz       |
| 333 Hz      | 2300 Hz       | 6000 Hz       |

Type: Variable

# ChanOptions (Output Frequency Units)

Used to determine if the measured frequency is output in units of Hz or Hz^2. This parameter is dimensioned to two for the CDM-VW300 and eight for the VWIRE 305 or CDM-VW305. For each element of the array, enter a ** 0**(Hz) or ** 1**(Hz^2) to configure the respective channel number.

Type: Variable

# Mult (Multiplier)

The multiplier that will be applied to the output for each channel. This parameter is dimensioned to two for the CDM-VW300 and eight for the VWIRE 305 or CDM-VW305. For each element of the array, enter the desired multiplier for the respective channel number.

Type: Constant

# Offset

The offset that will be applied to the output for each channel. This parameter is dimensioned to two for the CDM-VW300 and eight for the VWIRE 305 or CDM-VW305.

Type: Variable

# SteinA, SteinB, SteinC (Steinhart-Hart Equation Coefficients)

The coefficients to use for A, B, and C in the Steinhart-Hart equation for calculating the thermistor temperature (degrees Celsius) for each channel of the CDM device. This parameter is dimensioned to two for the CDM-VW300 and eight for the VWIRE 305 or CDM-VW305. For each element of the array, enter the desired coefficients for the respective channel number. To output Ohms from the thermistor, set the parameters to ** 0**.

Type: Variable

# RF_MeanBins (Rainflow Histogram Mean Axis Bins)

Specifies the number of bins for the Mean axis of the rainflow histogram. This parameter is dimensioned to two for the CDM-VW300 and eight for the VWIRE 305 or CDM-VW305. For each element of the array, enter the desired number of bins for the respective channel number. The minimum value is 1; the product of RF_MeanBins and RF_AmpBins cannot exceed 4096.

Type: Variable

# RF_AmpBins (Rainflow Histogram Amplitude Axis Bins)

Specifies the number of bins for the Amplitude axis of the rainflow histogram. This parameter is dimensioned to two for the CDM-VW300 and eight for the VWIRE 305 or CDM-VW305. For each element of the array, enter the desired number of bins for the respective channel number. The minimum value is 1; the product of RF_MeanBins and RF_AmpBins cannot exceed 4096.

Type: Variable

# RF_LowLim (Rainflow Histogram Range Lower Limit)

Specifies the lower limits of the histogram range for the rainflow histogram calculations. This parameter is dimensioned to two for the CDM-VW300 and eight for the VWIRE 305 or CDM-VW305. For each element of the array, enter the desired lower limit for the respective channel number.

Type: Variable

# RF_HighLim (Rainflow Histogram Range Upper Limit)

Specifies the upper limits of the histogram range for the rainflow histogram calculations. This parameter is dimensioned to two for the CDM-VW300 and eight for the VWIRE 305 or CDM-VW305. For each element of the array, enter the desired upper limit for the respective channel number.

Type: Variable

# RF_Hyst (Rainflow Hysteresis)

Sets the minimum amplitude of a cycle for it to be counted in the rainflow histogram. This parameter is dimensioned to two for the CDM-VW300 and eight for the VWIRE 305 or CDM-VW305. For each element of the array, enter the minimum amplitude for the respective channel number.

Type: Variable

# RF_Form (Rainflow Histogram Form)

Defines the form for the rainflow histogram. This parameter is dimensioned to two for the CDM-VW300 and eight for the VWIRE 305 or CDM-VW305. For each element of the array, enter a three-digit code in the format of ABC, based on the following:

| Code  | Form                                                |
| ----- | --------------------------------------------------- |
| A = 0 | Reset histogram after each output                   |
| A = 1 | Do not reset histogram                              |
| B = 0 | Divide bins by total count                          |
| B = 1 | Output total in each bin                            |
| C = 0 | Open form; include outside range values in end bins |
| C = 1 | Closed form; exclude values outside range           |

Type: Variable
