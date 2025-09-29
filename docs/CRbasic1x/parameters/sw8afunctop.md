# FunctOp

Determines the result returned by the SW8A. A numeric value is entered. Right-click the parameter to display a drop-down list box.

| Code | Function                                                                                                                                            |
| ---- | --------------------------------------------------------------------------------------------------------------------------------------------------- |
| 0    | Returns the state of the signal at the time the instruction is executed. A 0 is stored for low and a 1 is stored for high.                          |
| 1    | Returns the duty cycle of the signal. The result is the percentage of time the signal is high during the scan interval.                             |
| 2    | Returns a count of the number of positive transitions of the signal.                                                                                |
| 3    | Returns a value indicating the condition of the module: - positive integer = ROM and RAM are good - negative value = RAM is bad - zero = ROM is bad |

Type: Constant
