# Subinterval

The Subinterval parameter is used to determine how the standard deviation will be averaged. 0 can be entered to use every sample taken during the output period, or a subinterval value can be entered to process standard deviations from shorter intervals of the output interval. Averaging subinterval standard deviations minimizes the effects of meander under light wind conditions.

Standard deviation of horizontal wind fluctuations from sub-intervals is calculated as follows:

Whereis the standard deviation over the data storage interval andare sub-interval standard deviations.

A subinterval is specified as a number of scans. The number of scans for a subinterval is given by:

Desired subinterval (sec) / scan rate (sec)

If the scan rate is 1 sec and the DataInterval is 60 minutes, the standard deviation is calculated for all 3600 scans when the subinterval is 0. With a subinterval of 900 scans, the standard deviation is the average of the four subinterval standard deviations. The last subinterval is weighted if it does not contain the specified number of scans.
