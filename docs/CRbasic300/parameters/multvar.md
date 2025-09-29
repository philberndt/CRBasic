# MultVar (Multiplier Variable)

The MultVar parameter is the variable or variable array that will be populated with the computed Multiplier(s) from the calibration(s). This variable should also be used for the Multiplier parameter in the measurement instruction for which the calibration is being made. MultVar should be dimensioned to the same size as the MeasureVar variable array. The element of the array for the calibration is set by the Index parameter. If MultVar is equal to 0 or NAN at the start of calibration, it will be set to 1 during the calibration process.

Type: Variable
