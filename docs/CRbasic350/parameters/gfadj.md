# GFAdj (Gauge Factor Adjustment)

Not used for zero calibration. Enter 0.

For shunt calibration, this parameter is the variable or variable array that is populated with the computed gauge factors used in the StrainCalc instruction for computing the microstrain. It should be dimensioned large enough to hold values for all the elements in the MeasureVar array. GFAdj is set equal to GFRaw during the calibration process.

Type: Variable or variable array
