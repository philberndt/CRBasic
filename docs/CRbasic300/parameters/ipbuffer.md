# IPBuffer (Input Buffer)

If the socket is being opened for communication with a serial (non-PakBus

**Note:** PakBus is a proprietary communication protocol developed by Campbell Scientific to facilitate communications between Campbell Scientific devices. Similar in concept to IP (Internet Protocol), PakBus is a packet-switched network protocol with routing capabilities. A registered trademark of Campbell Scientific, Inc.

) device, enter a value for the size of the input serial buffer that should be created in datalogger memory. Set this parameter to 0 for communication with a PakBus device. When this parameter is set to 0, the port will be opened as a PakBus port and PakBus beaconing will take place every 3 minutes. For serial communications with non-PakBus devices, specify a buffer size large enough to hold the number of bytes that will arrive between serial input reads. Note that oversizing the buffer needlessly takes away from datalogger memory. For non-PakBus communication, do not set this parameter to 0 or PakBus beacons initiated by the datalogger may interfere with communications. For non-PakBus communication that does require a user-specified buffer (such as when used with [ModbusClient()](../Instructions/modbusclient.md), set this parameter to 1 so that beaconing does not occur.

Type: Constant
