# Source

The name of the first Variable that is the input for the instruction. If a two dimensional Level Crossing Histogram is desired, then the source must be at least a two dimensional array. The array element specified in the Source argument will be compared to the crossing levels (set in CrossingArray). The next element of the source array will be compared to the boundary levels set by the SecondArray.

The source value(s) may be the result of a measurement or calculation. When a DataTable having a Level Crossing instruction is called, the Source's first element is checked to see if its value has changed from the previous value. Only when the value of the first Source element crosses one or more of the levels set by the Crossing Array, is the count of one or more (depending upon how many levels are crossed) of the histogram bins incremented. The second Source element is compared to the values in the SecondArray only when a level crossing by the first source element has occurred.

Right-click the parameter to display a list of defined variables.

Type: Variable or Array
