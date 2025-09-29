# TDRFilterLevel

The TDR200 can reduce noise of the TDR trace by applying a weighted moving average (arithmetic convolution) to the data points. This parameter is used to determine the number of neighboring values to include in the weighted average. Acceptable values are 0 (no filtering) to 10 (maximum filtering). As an example, choosing a Filter Level of 2 will use 5 measured values in calculating the final output value: 2 values prior to the point of interest, the actual measured point of interest, and the 2 values after that point. Each of those points is weighted with the closer points having more weight than those further away.

Type: Constant
