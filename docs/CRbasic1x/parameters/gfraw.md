# GFRaw (Gauge Factor)

Zero calibration: Zero can be entered for this parameter (not used).

Shunt calibration: When setting up a Shunt Calibration, this is the variable or variable array that holds the raw gauge factor(s) for the strain gauges. It should be a different array than that used for the adjusted gauge factors in the StrainCalc instruction, and the value(s) should never be changed. This variable array must be dimension to the same size as the MeasureVar.

Type: Variable array
