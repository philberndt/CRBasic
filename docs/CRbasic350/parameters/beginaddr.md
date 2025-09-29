# BeginAddr (Beginning Address)

The PakBus address of the first destination datalogger with which the host will be communicating. If the addresses for the devices to be communicated with using this instruction are in sequential order, BeginAddr can be a scalar variable containing the address of the first device. If the addresses are not in sequential order, BeginAddr is an array declared as type Long, where each element of the array contains the PakBus address of a device to be communicated with.

Type: Variable or variable array containing an Integer, 1 through 4094

**NOTE:** By default,Campbell Scientificsoftware uses the following PakBus addresses: LoggerNet 4094, VisualWeather 4094, PC400 4093, PC200W 4092, PConnect/PConnectCE 4091, RTDAQ 4090, Device Configuration Utility 4089.
