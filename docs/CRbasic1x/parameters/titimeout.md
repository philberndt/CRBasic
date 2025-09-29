# Timeout

Used to determine the results to be returned when no changes have occurred in the specified ports since the last execution of the instruction. If the timeout period has expired and no change has occurred, a null value will be returned (0 for frequency and NaN for period or time since last edge). If the timeout period has not expired the last valid result will be returned. This is useful for measuring signals with periods greater than the scan interval. The maximum Timout is 2^32 x the scan. Typically, a timeout value is at least one scan interval. However, 0 can be entered to make the calculation each time the instruction is executed. A timeout less than the scan interval will default to the scan interval.

Type: Constant (or expression that evaluates as a constant) or Variable
