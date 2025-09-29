# CVO4Reps

Indicates the number of channels to set to the defined voltage or current. Additional SDM-CVO4 devices can be controlled by one SDMCVO4 instruction by assigning them consecutive addresses and setting the CVO4Reps parameter to a value equal to the total number of channels of all devices (e.g., to set all four channels on two devices, set the CVO4Reps parameter to 8).

If the CVO4Reps parameter is set to 0, power to the device will be turned off.

Type: Constant (or expression that evaluates as a constant)
