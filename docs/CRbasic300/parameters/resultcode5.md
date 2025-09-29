# ResultCode (Result Code)

The variable in which a response code for the transmission will be stored. Each time the Network instruction is executed, the ResultCode for each destination device is incremented by 1. When a response is received back from a destination device, the ResultCode is set to -1.

If the Reps parameter (the number of destination devices) is greater than one, then ResultCode should be dimensioned as an array large enough to accommodate a result from each of the destination devices.

Type: Variable or Variable Array
