# Count

Optional parameter that returns the actual number of values used to calculate the running value. The actual number of values used may be different than the number specified for the calculation due to NAN values in the buffer of historical data. The Count parameter can only be used if the optional Reset parameter is also used. The Count parameter is a variable of Type Long and must be the same dimension as the array that is being averaged (i.e., the same as reps). A count used for each rep will be written to this variable on each execution of the instruction.
