# CDM_VoltFilt (Measurement on Spectrum Module)

The SPECTRUM module is a 3- or 9-channel analog input module for the datalogger. It includes programmable anti-alias filtering and DC excitation. The CDM_VoltFilt() instruction is used to pass individual voltage measurements for each channel on a SPECTRUM to the datalogger CPU at the scan interval.

## Syntax

CDM_VoltFilt( Module, Addr,Dest,Reps, Range, Chan, FiltOption, Excitation,Mult, Offset)

For measurements faster than 1KHz, a subscan must be used. This is done by specifying the desired scan rate and adding a CDM_VoltFIlt instruction inside a subscan such as:

```
Scan(2, msec,125,0)
SubScan(500,usec,4)
CDM_VoltFilt(SPECTRUM103,EPI_ADDR,Spectrum_vals,NUM_Reps,mV5000,1,4,2,1.0)
CallTable TSData
NextSubScan
NextScan
```

This specifies a main scan rate of 500 HZ and the spectrum output at 2KHz. When a subscan is used, the subscan interval determines the output rate. The number of iterations must align with the specified rate in the scan instruction. (htat is, subscan rate \* iterations must equal the scan rate).

For scan rates slower than 10 Khz, a subscan can be used if measurements slower than the main scan rate are desired. For example: We would like to measure channels on the Spectrum at 200 Hz but have several other measurements we would like to read at 1 Hz. This can be accomplished using a subscan to read the Spectrum values while reading the other measurements at the 1 Hz Rate.

```
Scan(1, sec,1250,0)
SubScan(5,msec,200)
CDM_VoltFilt(SPECTRUM103,EPI_ADDR,Spectrum_vals,NUM_Reps,mV5000,1,4,2,1.0)
CallTable TSData
NextSubScan
'Other measurements at 1Hz
NextScan
```

## Remarks

The Spectrum Module is a 3 or 9 channel analog input module for the CR1000X, CR6, GRANITE6, GRANITE 9, or GRANITE 10 datalogger. It includes programmable anti-alias filtering and DC excitation. The program scan interval determines the filter module output interval. Data are passed from the filter module to the datalogger CPU over the EPI or CPI bus for processing and final storage at this scan interval. The filter module collects alias-free, 10-kHz samples from each of its 3 or 9 analog to digital converters, applies additional real-time, finite-impulse-response filtering, and down-samples the 10-kHz data to the programmed scan rate. The filter module supports the following scan intervals:

| Scan Interval | Scan Rate |
| ------------- | --------- |
| 100 μsec      | 10 kHz    |
| 200 μsec      | 5 kHz     |
| 500 μsec      | 2 kHz     |
| 1 msec        | 1 kHz     |
| 2 msec        | 500 Hz    |
| 5 msec        | 200 Hz    |
| 10 msec       | 100 Hz    |
| 20 msec       | 50 Hz     |
| 50 msec       | 20 Hz     |
| 100 msec      | 10 Hz     |
| 200 msec      | 5 Hz      |
| 500 msec      | 2 Hz      |
| 1000 msec     | 1 Hz      |

## Parameters

# Module (Module)

Specifies the type of module to configure. Valid options are SPECTRUM103 or SPECTRUM109.

Type: Constant

# Addr (Address)

Specifies the CPI or EPI bus and the CPI address of the module. The valid CPI address range is 1 through 120. If no bus is specified, then CPI_BusA is used by default.

| Description       |
| ----------------- |
| CPI_BusA + 1..120 |
| CPI_BusB + 1..120 |
| EPI_Bus + 1..120  |

**NOTE:** The two EPI buses are linked internally, so there is no A and B designation for the EPI ports.

Type: Constant

# Dest (Destination)

The Variable in which to store the results of the instruction. Right-click the parameter to display a list of defined variables.

If this instruction has a Repetitions parameter and it is greater than 1, the results are stored in an array with the variable name. The array must be dimensioned large enough to hold all of the values returned from all of the Reps.

Type: Variable or Array

# Reps (Repetitions)

The number of times the measurement should be made. Measurements are made on consecutive channels. If the Reps parameter is greater than 1, the Dest parameter must be a variable array.

Type: Constant integer (or expression that evaluates as a constant).

# Range (Voltage Range)

The expected voltage range of the input from the sensor. Valid options are:

| Alphanumeric | Description |
| ------------ | ----------- |
| mV10000      | 10000 mV    |
| mV5000       | 5000 mV     |
| mV1000       | 1000 mV     |
| mV200        | 200 mV      |

Type: Constant

# Chan (Channel)

The number of the channel on which to make the first measurement. If the Reps parameter is greater than 1, additional measurements will be made on sequential channels.

Type: Constant

# FiltOption (Filter Option)

A constant designating the sample ratio for the anti-aliasing filter that will be applied prior to performing the FFT. The sample ratio determines the top of the pass-band (FPASS) and the beginning of the stop-band (FSTOP) of the anti-aliasing filter relative the sample rate (FSAMPLE). The sample rate is the inverse of the scan interval in the CRBasic program. FiltOption must be the same for all channels of a single SpectrumFilter Module. The Granite Spectrum supports the following sample ratios, pass-bands, and stop-bands:

| Sampling Ratio | Fpass      | Fstop         |
| -------------- | ---------- | ------------- |
| 4              | Fsample/4  | Fsample/2     |
| 20             | Fsample/20 | Fsample/3.333 |

Type: Constant

# Excitation (Excitation)

Used to set the continuous DC output level for the excitation channel(s). If the Reps parameter is greater than 1, the same excitation level will be used for each channel. A numeric code is entered:

| Code | Output Level             |
| ---- | ------------------------ |
| 0    | Disabled (No Excitation) |
| 1    | Constant 10 mA           |
| 2    | Constant 5 V             |
| 3    | Constant 10 V            |

Type: Constant

# Mult, Offset (Multiplier and Offset)

Factors by which to scale the raw results of the measurement. Typically used to convert the raw measurement to engineering units or to units other than which is output. For example, the TCDiff instruction measures a thermocouple and outputs temperature in degrees C. A multiplier of 1.8 and an offset of 32 will convert the temperature to degrees F.

For temperature measurements, a multiplier (mult) of 1 and an offset of 0, would output in degrees Celsius. For analog measurements, a multiplier (mult) of 1 and an offset of 0, would output the measured voltage in millivolts divided by the excitation voltage in volts.

If Repetitions of greater than 1 are used for this instruction, Repetitions can also be used for the Multiplier and Offset.[SeeMultipliers, Offsets, and Disable Variables with Repetitionsfor more information.](../Info/multipliersoffsets.md)

Type: Constant, Variable, Array, or Expression
