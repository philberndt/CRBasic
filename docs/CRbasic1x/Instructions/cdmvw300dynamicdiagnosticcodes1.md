# VWIRE or CDM-VW300 Dynamic Diagnostic Codes

Diagnostic codes are returned with each dynamic frequency measurement made by the VWIRE 305 or CDM-VW300 series device. The results are stored in the third parameter. For additional information on controller area network peripherals and Campbell modules, see [CPI and CDM](https://www.campbellsci.com/news-cpi-cdm).

Each diagnostic code contains five diagnostic parameters pertaining to each measurement:

1. Excitation Strength
2. Low Amplitude Warning
3. High Amplitude Warning
4. Low Frequency Warning
5. High Frequency Warning

**Excitation Strength ** is a value in the range 0 to 255 which represents the sensor excitation, between 0 and 6 volts, chosen by the device to keep the sensor s wire vibrating. The excitation strength cannot be specified explicitly. Instead, the desired amplitude response of the sensor s wire is specified. For each measurement, the VWIRE 305 or CDM-VW300 calculates what excitation strength is needed in order to keep the sensor s wire vibrating at the specified level.

The value returned should be divided by 42.5 to provide an excitation level expressed in units of Volts (0V to 6V). For example:

|     |                     |
| --- | ------------------- |
| 255 | 6 Volts             |
| 180 | 4.235 Volts         |
| 100 | 2.353 Volts         |
| 35  | 0.824 Volts, 824 mV |

For scenarios where the CPU processing of the datalogger needs to be minimized, a floating point multiplication operation is more efficient than a division operation. Multiplying the Excitation Strength by 0.02353 will provide essentially the same result as dividing by 42.5 and will increase the CPU efficiency.

A**Low Amplitude warning ** is given when the response amplitude of the wire vibrating in the sensor falls to or below 50% of the specified level (i.e., the resonant amplitude as set in the CDM_VW300Config instruction). The level must be greater than 50% to ensure an accurate measurement of the frequency. Although a frequency output may be provided when this flag is set, its value should be interpreted with caution.

**NOTE:** Consider the [CDM_VW300Config](cdmvw300config2.md) instruction`SysOptions`parameter to have the device automatically change the frequency toNAN

**Note:** Not a number. A data word indicating a measurement or processing error. Voltage overrange, SDI-12 sensor error, and undefined mathematical results can produce NAN.

whenever a diagnostic parameter is flagged as true.

A **High Amplitude warning** is given when the response amplitude of the wire vibrating in the sensor reaches or exceeds 200% of the specified level (i.e., the resonant amplitude as set in the CDM_VW300Config instruction). The level must be less than 200% to ensure an accurate measurement of the frequency. Although a frequency output may be provided when this flag is set, its value should be interpreted with caution.

The VWIRE 305 and CDM-VW300 series devices do not report a Low Amplitude warning and a High Amplitude warning for the same measurement result on any particular channel. Only one of these warnings (or none) will be given for a measurement.

A **Low Frequency warning** is given when the frequency of the wire vibrating in the sensor falls below the lower window limit set using the F_Low parameter in the CDM_VW300Config instruction, but remains above the actual low frequency limit selected for use by the device. When a frequency for the F_Low setting is entered, the device cannot comply exactly with the value given, since it is constrained to use integer multiples of only certain frequencies based on the current sample rate (see following chart). Because of this, there is a difference (a window) between the specified F_Low value and the actual lowest frequency used by the device for the low frequency cutoff (filtering). When the reading of the sensor falls between these two levels, the Low Frequency flag will be set.

| Scan Rate | Boundary Resolution(F_Low or F_High must be an integer multiple of this frequency) |
| --------- | ---------------------------------------------------------------------------------- |
| 20 Hz     | 47.68 Hz                                                                           |
| 50 Hz     | 95.37 Hz                                                                           |
| 100 Hz    | 190.73 Hz                                                                          |

Although a frequency reading may be provided when this flag is set, its value should be interpreted with caution. Consider the [CDM_VW300Config](cdmvw300config2.md) instruction`SysOptions`parameter to have the device automatically change the frequency toNAN

**Note:** Not a number. A data word indicating a measurement or processing error. Voltage overrange, SDI-12 sensor error, and undefined mathematical results can produce NAN.

for this case.

A **High Frequency warning** is given when the frequency of the wire vibrating in the sensor rises above the upper window limit set using the F_High parameter but remains below the actual high frequency limit selected for use by the device. As with the F_Low parameter, the device cannot comply exactly with the value given, and there will be a difference (a window) between the specified F_High value and the actual highest frequency used by the device for the high frequency cutoff (filtering). When the reading of the sensor falls between these two levels, the High Frequency flag will be set.

Although a frequency reading may be provided when this flag is set, its value should be interpreted with caution.Consider the [CDM_VW300Config](cdmvw300config2.md) instruction`SysOptions`parameter to have the device automatically change the frequency toNAN

**Note:** Not a number. A data word indicating a measurement or processing error. Voltage overrange, SDI-12 sensor error, and undefined mathematical results can produce NAN.

for this case.

The device does not report a Low Frequency warning and a High Frequency warning for the same measurement result on any particular channel. Only one or the other of these warnings (or none) will be given for any one measurement.

## Decoding the Diagnostic Code into 5 Parameters

The five diagnostic parameters for one frequency reading are combined into one CRBasic Long integer value (a 32-bit unsigned integer). This helps to minimize the amount of data being sent over the CPI Bus, thus, maximizing measurement throughput rates.

In order to recover the 5 diagnostic parameters from a diagnostic code, the individual bit patterns inside the Long integer must be considered. The 12 lowest order bits of the integer are used to encode the 5 diagnostic parameters.

### Excitation Strength

The 8 lowest order bits (2^0 through 2^7) of the integer represent the Excitation Strength (0 - 255). Under normal circumstances (when no Low/High Amplitude or Low/High Frequency warnings are given) the diagnostic code itself will return a value in the range 0 to 255. However, if any of the amplitude or frequency warning flags are set, the value will be larger than 255. In order to strip out the effect of any bits higher than the lowest 8 bits (i.e,. strip out the warning flags), a bit masking operation should be performed.

One way to accomplish this is to use the AND operator in CRBasic:

ExciteStrengthRaw = DiagCode AND 255

where`ExciteStrengthRaw`has a long integer data type. This ensures that only the 8 lowest order bits are considered, resulting in a value that is always in the range 0 to 255. Divide this number by 42.5 to obtain the excitation strength in units of Volts:

ExciteStrengthV = ( DiagCode AND 255) / 42.5

where`ExciteStrengthV`has a float/IEEE4

**Note:** Four-byte, floating-point data type. IEEE Standard 754. Same format as Float.

data type.

### Low Amplitude

The 9th lowest order bit (2^8 or 256) corresponds to the Low Amplitude warning flag. The following expression can be used to isolate the state of this bit:

(DiagCode AND 256)

This expression will return zero (0) if the bit is not set (i.e., no Low Amplitude warning) and will return 256 if the bit is set (i.e., Low Amplitude Warning reported). Since many diagnostic codes are given each second, it may make sense to use a counter to track occasional instances of this flag being set:

If (DiagCode AND 256) Then LowAmpCount += 1

where`LowAmpCount`has an integer data type. An alternative is to assign the expression to aBoolean

**Note:** Data type used to represent conditions or hardware that have only two states (true or false) such as flags and control ports.

value, though it would be hard to see the warning if it turns on and off too rapidly:

LowAmpWarning = (DiagCode AND 256)

where`LowAmpWarning`is aBoolean

**Note:** Data type used to represent conditions or hardware that have only two states (true or false) such as flags and control ports.

flag/data type.

### High Amplitude

The 10th lowest order bit (2^9 or 512) corresponds to the High Amplitude warning flag, and can be isolated with this expression:

`(DiagCode AND 512)`

Similar techniques to those described above for Low Amplitude can be used with this expression.

### Low Frequency

The 11th lowest order bit (2^10 or 1024) corresponds to the Low Frequency warning flag, and can be isolated with this expression:

(DiagCode AND 1024)

Similar techniques to those described above for Low Amplitude can be used with this expression.

### High Frequency

The 12th lowest order bit (2^11 or 2048) corresponds to the High Frequency warning flag, and can be isolated with this expression:

(DiagCode AND 2048)

Similar techniques to those described previously for Low Amplitude can be used with this expression.

The Low Amplitude and High Amplitude flags will not be reported by the VWIRE 305 or CDM-VW300 as both set at the same time for the same measurement. Similarly, the Low Frequency and High Frequency flags will not be reported by the VWIRE 305 or CDM-VW300 as both set at the same time for the same measurement. However, the Low Amplitude flag may be set along with the Low or High Frequency Flag, or the High Amplitude flag may be set along with the Low or High Frequency Flag.

## Interpreting the Diagnostic Code Based on Range

Although the above techniques constitute the preferred way for processing and communicating the state of the diagnostic parameters, it may be useful to examine the ranges of values within which the returned code itself falls.

The following chart summarizes these ranges and their meaning:

| Minimum Excitation 0 V (Low)             | Maximum Excitation 6 V (High) |
| ---------------------------------------- | ----------------------------- | ---- |
| Normal Amplitude and Frequency Range     | 0                             | 255  |
| Low Amplitude Only Range                 | 256                           | 511  |
| High Amplitude Only Range                | 512                           | 767  |
| Low Frequency Only Range                 | 1024                          | 1279 |
| Low Amplitude with Low Frequency Range   | 1280                          | 1535 |
| High Amplitude with High Frequency Range | 1536                          | 1791 |
| High Frequency Only Range                | 2048                          | 2303 |
| Low Amplitude with High Frequency Range  | 2304                          | 2559 |
| High Amplitude with High Frequency Range | 2560                          | 2815 |
