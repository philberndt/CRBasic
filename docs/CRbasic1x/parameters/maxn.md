# MaxN (Maximum Number)

The maximum number of values the datalogger should maintain in memory for the instruction, from which the median will be calculated. This memory is set up as ring memory, meaning old values will be overwritten with new values if the number of values stored before output is greater than MaxN. Thus, the last MaxN values will be used to calculate the output.

Type: Constant
