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
