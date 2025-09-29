# Range

The voltage range for the measurement. An alphanumeric code is entered. Right-click the parameter to display a list or enter the code directly.

| Code       | Description                                                                                                                           |
| ---------- | ------------------------------------------------------------------------------------------------------------------------------------- |
| mV5000     | +5000 mV                                                                                                                              |
| mV1000     | +1000 mV                                                                                                                              |
| mv200      | +200 mV                                                                                                                               |
| Autorange  | Datalogger tests for and uses most suitable range.                                                                                    |
| AutorangeC | Autorange, checks for open input.                                                                                                     |
| mV5000C    | +5000 mV, checks for open input, sets excitation to ~5600mV. Overrange values may go undetected; use code to detect overrange values. |
| mV1000C    | +1000 mV, checks for open input, sets excitation to ~1250 mV.                                                                         |
| mV200C     | +200 mV, checks for open input, sets excitation to ~1250 mV.                                                                          |

For signals that do not fluctuate too rapidly, AutoRange allows the datalogger to automatically choose the input voltage range. AutoRange results in two voltage measurements being performed. The first voltage measurement is done quickly at a first notch frequency (fN1) of 50 kHz, the result of which determines the input voltage range to use for the second measurement. The second measurement is then performed using the range determined from the first measurement, along with the fN1chosen for the measurement instruction. Both measurements use the settling time chosen for the particular measurement instruction. Auto-ranging optimizes resolution but takes longer than a fixed range measurement because of the two-measurement sequence. The exception to this two-measurement sequence is if Reps are made on the same channel (Reps parameter is negative). In this instance, the test measurement is made only once. Subsequent repetitions are made with the delay between each measurement being the specified settling time. Autorange should not be used if fast measurements are required or if the analog signal could change significantly over the course of the measurement.

The C options check for an open connection on the analog input by applying a short overranging test signal to the analog input prior to making the measurement. For a voltage range of mv5000C,5.6Vis applied. For a voltage range of mv1000C or mv200C, 1.25V is applied. An open (broken) sensor will result in a measurement overrange, clearly indicating a sensor problem instead of returning a bad measured value caused by a floating input.

The open-detect option may not work properly in the presence of external leakage (< 1 MOhm) to ground, as the overrange test signal could discharge through the external leakage during the input settling time such that an overrange condition no longer exists. In this case the measurement settling time should be minimized to minimize the discharge time of the overrange test signal.

The open circuit detection (C option) can cause measurement errors for slow responding sensors, as such sensors may not have sufficient time to recover from the applied short duration (50 microseconds) test signal. For such sensors, increasing the measurement settling time beyond the default may be necessary for sufficient sensor recovery time. If measurement speed is critical with a slow responding sensor, then it may be preferable to not use the open detect (C option).

Type: Constant
