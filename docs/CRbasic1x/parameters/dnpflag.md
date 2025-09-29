# DNPFlag (DNP Flag)

A constant, expression, variable, or variable array that indicates the value of the DNP3 data quality flag corresponding to the Source parameter. If a constant is used, then by default, all quality flags will be set to online, regardless of the value entered (for example, 0 or 1 both evaluate to online ). If a variable or variable array is used, the variable must be declared as a data type of Long, and the DNP3 flags will reflect the value of the variable or variable array. The variable or variable array can then be set under program control. A 1 sets a quality flag to online; a 0 sets a flag to offline.

If the array for the flag parameter is smaller than the array for the source parameter, then the last element of the flag array is used for all of the remaining source array elements. A single variable (scalar) is treated like an array of one element.

Type: Variable or Constant
