# MinAmp (Minimum Amplitude)

The minimum amplitude that a stress/strain cycle must have to be counted. The MinAmp's value should be less than the band size of the amplitude columns ( [HiLim - LoLim]/[AmpBins] ) or else cycles having the amplitude specified for the first column of bins will not be counted. If the MinAmp's value is set too small, processing time will be consumed counting cycles which are in reality just noise.

Type: Constant (or expression that evaluates as a constant)
