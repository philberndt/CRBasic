# SysOptions (System Options)

Determines if a numeric value orNAN

**Note:** Not a number. A data word indicating a measurement or processing error. Voltage overrange, SDI-12 sensor error, and undefined mathematical results can produce NAN.

will be stored when a flag condition occurs, and whether or not the diagnostic LEDs are on or off. The following are valid options:

| Code | Description                                                                |
| ---- | -------------------------------------------------------------------------- |
| 0    | Display numeric output when diagnostic flags are set; LEDs are operational |
| 1    | Display NANs when diagnostic flags are set; LEDs are operational           |
| 10   | Display numeric output when diagnostic flags are set; LEDs are disabled    |
| 11   | Display NANs when diagnostic flags are set; LEDs are disabled              |

Disabling the LEDs offers some measure of power conservation.

Type: Variable
