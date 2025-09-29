# Reps (Repetitions)

The Reps parameter is a constant or variable used to specify how many values in the array to calibrate. Reps must be equal to either 1 or the size of the MeasureVar parameter array. When Reps is equal to the size of the MeasureVar array, all elements of the MeasureVar array will be calibrated in a single scan (the Index parameter must be set to 1). When Reps is set to 1, a single element of the MeasureVar array, specified by the Index parameter will be calibrated.

If the Reps parameter is declared as a variable, the number of reps can be changed during program operation. This allows the calibration of a complete array or a single element at different points in time. Reps should be initialized to either 1 or the size of the MeasureVar array prior to starting a calibration. If Reps is set to zero, no calibration will occur for this instruction.

Type: Constant or Variable
