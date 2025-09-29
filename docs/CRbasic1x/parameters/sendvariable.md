# SendVariable (Variables to Send)

The variable(s) that will be sent from this datalogger to the destination datalogger(s). If sending a Public variable that has been Aliased, either the Alias or the original variable name can be used. If multiple values are being sent to multiple destination dataloggers, this parameter must be a multi-dimensional array containing the values to be sent to each destination device.

For instance, if you are sending 2 values to each of 3 destination devices, the variable should be dimensioned to (3, 2). The results would be:

| Destination device #, Value = Send From | Destination device #, Value = Send From |
| --------------------------------------- | --------------------------------------- |
| Destination device 1, value 1 = (1, 1)  | Destination device 1, value 2 = (1, 2)  |
| Destination device 2, value 1 = (2, 1)  | Destination device 2, value 2 = (2, 2)  |
| Destination device 3, value 1 = (3, 1)  | Destination device 3, value 2 = (3, 2)  |

Type: Variable or variable array
