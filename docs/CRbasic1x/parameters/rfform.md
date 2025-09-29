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
