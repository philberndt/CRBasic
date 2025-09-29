# Form

The Form argument consists of three elements: ABC. Open form includes values > UpLim in the upper bin and values < LoLim +NAN

**Note:** Not a number. A data word indicating a measurement or processing error. Voltage overrange, SDI-12 sensor error, and undefined mathematical results can produce NAN.

s in the lower bin. Closed form excludes values outside of the limits (including NANs). Right-click within the parameter to display a list.

| Code  | Form                                                           |
| ----- | -------------------------------------------------------------- |
| A = 0 | Reset histogram values to 0 after each output.                 |
| A = 1 | Do not reset histogram.                                        |
| B = 0 | Divide bins by total count.                                    |
| B = 1 | Output total in each bin.                                      |
| C = 0 | Open form. Include values outside LoLim and UpLim in end bins. |
| C = 1 | Closed form. Exclude values outside range.                     |

Type: Constant
