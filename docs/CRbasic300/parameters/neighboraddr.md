# NeighborAddr (PakBus Address to Neighbor Datalogger)

Used to specify a static route to the destination datalogger (for example, thePakBus

**Note:** PakBus is a proprietary communication protocol developed by Campbell Scientific to facilitate communications between Campbell Scientific devices. Similar in concept to IP (Internet Protocol), PakBus is a packet-switched network protocol with routing capabilities. A registered trademark of Campbell Scientific, Inc.

address of a "neighbor" datalogger that the host can go through to communicate with the destination datalogger).

If 0 is entered, the destination device is assumed to be a neighbor (i.e., the host datalogger can communicate with the destination directly). If a non-valid PakBus address is entered (a negative number or a number greater than 4094), the route to the destination device will be "autodiscovered" by other means in the PakBus network (such as beaconing or a Hello messages). If the instruction has a ResultCode parameter, an error code is returned until the route is discovered. When autodiscovery is used, the COMPort parameter is ignored.

Type: Integer between 1 and 4094

**NOTE:** By default,Campbell Scientificsoftware uses the following PakBus addresses: LoggerNet 4094, VisualWeather 4094, PC400 4093, PC200W 4092, PConnect/PConnectCE 4091, RTDAQ 4090, Device Configuration Utility 4089.
