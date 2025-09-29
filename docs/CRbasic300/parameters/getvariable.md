# GetVariables (Variables to Retrieve)

The variable or variable array in which values retrieved from the destination datalogger will be stored. If multiple values are being retrieved from multiple dataloggers, this parameter must be dimensioned to a multi-dimensional array large enough to accommodate the values returned. For example, if 2 values are being returned by each of 4 destination devices, the variable should be dimensioned to (4, 2).

| Destination device #, Value = Store In | Destination device #, Value = Store In |
| -------------------------------------- | -------------------------------------- |
| Destination device 1, value 1 = (1, 1) | Destination device 1, value 2 = (1, 2) |
| Destination device 2, value 1 = (2, 1) | Destination device 2, value 2 = (2, 2) |
| Destination device 3, value 1 = (3, 1) | Destination device 3, value 2 = (3, 2) |
| Destination device 4, value 1 = (4, 1) | Destination device 4, value 2 = (4, 2) |

Type: Variable or variable array
