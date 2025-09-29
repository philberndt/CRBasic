# Mode (Current State)

A variable that stores an integer that indicates the current state of the calibration. States are as follows:

| Value Returned | State                                                                                                                                                                                                                         |
| -------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| -1             | Error in the calibration setup                                                                                                                                                                                                |
| -2             | Multiplier set to 0 or =NAN **Note:** Not a number. A data word indicating a measurement or processing error. Voltage overrange, SDI-12 sensor error, and undefined mathematical results can produce NAN. ; measurement = NAN |
| -3             | Reps is set to a value other than 1 or the size of MeasureVar                                                                                                                                                                 |
| 0              | No calibration                                                                                                                                                                                                                |
| 1              | Ready to calculate                                                                                                                                                                                                            |
| 2              | Working                                                                                                                                                                                                                       |
| 3              | Ready to enter the shunt resistance into KnownRs (only for shunt calibration)                                                                                                                                                 |
| 4              | Ready to calculate                                                                                                                                                                                                            |
| 5              | Working (only applicable for two point calibration)                                                                                                                                                                           |
| 6              | Calibration complete                                                                                                                                                                                                          |

Type: Variable
