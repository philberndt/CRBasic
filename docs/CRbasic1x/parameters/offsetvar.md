# OffsetVar (Offset Variable)

The OffsetVar parameter is the variable or variable array that will be populated with the computed Offset(s) from the calibration(s). This variable should also be used for the Offset parameter in the measurement instruction for which the calibration is being made. When performing a Multiplier Only Two Point calibration, zero can be entered for this parameter. When performing any other calibration function, the OffsetVar should be dimensioned to the same size as the MeasureVar variable array. The element of the array for the calibration is set by the Index parameter. If OffsetVar is equal to NAN at the start of calibration, it will be set to 0 during the calibration process.

Type: Variable
