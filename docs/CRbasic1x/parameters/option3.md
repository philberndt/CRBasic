# Option

The Option argument can program the level crossing histogram to output the actual number of times that the signal crossed the level associated with an individual bin, or it can be set to output the fraction of crossings associated to that bin with respect to the total number of crossings (number of crossings associated with an individual bin divided by the total number of crossings associated with all of the bins). The option argument is also used to program the histogram to count on either the rising or falling edge, and whether or not to reset the histogram after each output. The Option Codeconsists of three elements: ABC. Right-click the parameter to display a list.

| Code  | Form                                                            |
| ----- | --------------------------------------------------------------- |
| A = 0 | Count on falling edge.                                          |
| A = 1 | Count on rising edge.                                           |
| B = 0 | Reset histogram counts to 0 after each output.                  |
| B = 1 | Do not reset histogram counts.                                  |
| C = 0 | Divide count in each bin by total number of counts in all bins. |
| C = 1 | Output total counts in each bin.                                |

Type: Constant
