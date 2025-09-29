# SampleInterval (Sample Interval)

In normal operation (non-burst mode), this is the datalogger scan rate, or the time series data interval.

If the measurement instruction that created the time-series data used burst mode (that is, the channel was entered as a negative number), theCR1000Xburst measurement samplingfrequencyis determined by the fN1(first notch frequency) parameterwhich can go up to a maximum of 31250 Hz (minimum sample interval of 32 uS). The SettlingTime parameter is used to delay once prior to beginning the burst. The total time prior to beginning the burst is the SettlingTime plus the ADC flush time (which is 450 uS). The sample interval resolution is 1/31250 Hz. The specified notch frequency will use the nearest multiple of (1/31250 Hz) to get as close to the specified frequency as possible.

Type: Constant (or expression that evaluates as a constant)
