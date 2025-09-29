# KnownRs (Known Resistance)

The KnownRs parameter is a variable array that holds the set point value(s) to be used in the calibration routine. 0 can be entered when performing a Zero calibration. When performing any other calibration function, the KnownVar must be dimensioned to the size of MeasureVar. If Reps is set to 1, the element of the array used for the calibration is set by the Index parameter.

Enter the resistance value as a positive number if shunting across the arm that holds the strain gauge for function 13, the arm that holds the gauge that is parallel to +ε for function 33, or the arm that holds the gauge that is parallel to to +ε for function 43. Enter the resistance value as a negative number if shunting across the arm that holds the completion resistor for function 13, the arm that holds the gauge that is parallel to -ε for function 33, or the arm that holds the gauge that is parallel to -ε for function 43.

Type: Variable
