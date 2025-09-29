# ScanMode

Specifies the number of values to be retrieved for scan-specific data. Normally the ScanMode parameter corresponds to the TGA100A "number of ramps" parameter that specifies how many absorption lines are being measured. If ScanMode is set to a lower number than the TGA100A "number of ramps" parameter, the data for ramp B and/or C will not be retrieved from the TGA100A. If ScanMode is set to a higher number than the TGA100A "number of ramps" parameter, the TGA100A will return zero for the ramp B and/or C values.

Type: Constant
