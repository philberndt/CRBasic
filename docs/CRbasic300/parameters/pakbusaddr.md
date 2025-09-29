# PakBusAddr (PakBus Address)

The PakBus address of the remote datalogger from which records should be accepted. If 0 is entered, the datalogger will accept records from any PakBus address.

Type: Integer between 1 and 4094

**NOTE:** By default,Campbell Scientificsoftware uses the following PakBus addresses: LoggerNet 4094, VisualWeather 4094, PC400 4093, PC200W 4092, PConnect/PConnectCE 4091, RTDAQ 4090, Device Configuration Utility 4089. 4095 is a broadcast address that can be used in a limited number of instructions. Typically, dataloggers are assigned addresses less than 4000.
