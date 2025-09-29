# Range

The expected input from the sensor. To choose the appropriate range, multiply the expected current by 10 (ohms).

Right-click the parameter to display a list or enter the code directly.

| Code      | Description                                       |
| --------- | ------------------------------------------------- |
| mV1000    | +1000 mV                                          |
| mv200     | +200 mV                                           |
| Autorange | Datalogger tests for and uses most suitable range |

For signals that do not fluctuate too rapidly, AutoRange allows the datalogger to automatically choose the input voltage range. AutoRange results in two voltage measurements being performed. The first voltage measurement is done quickly at a first notch frequency (fN1) of 50 kHz, the result of which determines the input voltage range to use for the second measurement. Autorange should not be used if fast measurements are required or if the analog signal could change significantly over the course of the measurement.

Type: Constant
