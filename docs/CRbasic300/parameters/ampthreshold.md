# AmpThreshold (Amplitude Threshold)

An optional parameter that is used to define a minimum value, in millivolts, for the amplitude of the signal. If an amplitude of less than the threshold is returned, NAN will be stored for the Frequency measurement (Dest(1)). If AmpThreshold is omitted, a default value of 0.01 mV is used. If a value of less than 0.01 mV is entered for this parameter, the precompiler will return an error. For best results, use an amplitude threshold that is an integer multiple of 0.0625 (for additional information, refer to the AVW200 manual).

Type: Constant
