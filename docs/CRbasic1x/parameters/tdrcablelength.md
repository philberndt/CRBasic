# CableLength

Specifies the cable length, in meters, of the TDR probes. If a 0 is entered for the Option parameter, cable length is used by the analysis algorithm to begin searching for the TDR probe. If a 1 or 2 is entered for the Option parameter, cable length is the distance to the start of the collected waveform.

The value used for CableLength is best determined using PCTDR with the Vp = 1.0. Adjust the CableLength and WindowLength values in PCTDR until the probe reflection can be viewed. Subtract about 0.5 meters from the distance associated with the beginning of the probe reflection.

Note that if CableLength is a scalar the specified CableLength applies to all probes read by this instruction; therefore, all probes must have the same cable lengths. However, CableLength can be a variable array loaded with the cable length values to use for each probe; e.g., CableLength(1) is used for the first probe, CableLength(2) is use for the second probe, etc. In this instance, the CableLength array should be loaded with values prior to the start of the scan.

Type: Constant or Variable Array
