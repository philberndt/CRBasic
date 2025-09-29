# Mode (Current State)

A variable that stores an integer that indicates the current state of the calibration. This is both a trigger value to start a process and a status value reporting an operational state or error state. States are as follows:

| Value Returned | State                                                                                                                                                                                                                         |
| -------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| -1             | Error in the calibration setup                                                                                                                                                                                                |
| -2             | Multiplier set to 0 or =NAN **Note:** Not a number. A data word indicating a measurement or processing error. Voltage overrange, SDI-12 sensor error, and undefined mathematical results can produce NAN. ; measurement = NAN |
| -3             | Reps is set to a value other than 1 or the size of MeasureVar                                                                                                                                                                 |
| 0              | No calibration                                                                                                                                                                                                                |
| 1              | Ready to calculate (KnownVar holds the first of a two point calibration)                                                                                                                                                      |
| 2              | Working                                                                                                                                                                                                                       |
| 3              | First point done (only applicable for two point calibration)                                                                                                                                                                  |
| 4              | Ready to calculate (KnownVar holds the second of a two point calibration)                                                                                                                                                     |
| 5              | Working (only applicable for two point calibration)                                                                                                                                                                           |
| 6              | Calibration complete                                                                                                                                                                                                          |
| -6             | New calibration attempted before datalogger is ready for calibration (one full scan must execute after calibration is complete, and before another calibration can be started)                                                |

If multiple FieldCal instructions are used in the program, you must define a Mode variable for each instruction.

Type: Variable
