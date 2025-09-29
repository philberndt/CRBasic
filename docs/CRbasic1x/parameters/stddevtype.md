# StdDevType

Optional parameter added in OS8.00and newer. Set this option to 1 to calculate a sample standard deviation instead of the default population standard deviation. The sample standard deviation uses a divisor of N−1, while the population standard deviation uses a divisor of N. A population standard deviation is used when you have data for the entire population. A sample standard deviation is used when you have data from a sample, not the entire population.

The following equation is used to calculate the default, population StdDev:

In contrast, the formula for the sample standard deviation is:

Note: the preceding optional parameters (RunReset, Count, TotalCall, and Call_ID) must be set in order to use the StdDevType option. The Reset parameter should be 0 (false), Count (N), should be a variable destination,TotalCall is 1, and Call_ID is any user-specified integer. See [TotalCalls Call_ID](TotalCalls_CallID_Explanation.md) for more information.See the **Sample Standard Deviation example program** for correct syntax.
