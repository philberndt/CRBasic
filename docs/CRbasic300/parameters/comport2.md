# ComPort (Communications Port)

The COM port that will be used by the instruction. Right-click to display a list.

| Alphanumeric | Description                                                                      |
| ------------ | -------------------------------------------------------------------------------- |
| 0            | Modbus packets routed via PakBus (using Datagram protocol; application ID = 502) |
| ComUSB       | USB port of the datalogger                                                       |
| ComRS232     | RS232 port of the datalogger                                                     |
| Com1         | Datalogger's control ports 1 (TX) & 2 (RX)                                       |
| ComRF        | Integrated RF communication                                                      |
| 502 - 65535  | Modbus TCP/IP, for data requests via Modbus TCP/IP protocol                      |

When communicating over TCP/IP, use the variable returned by the TCPOpen function for the ComPort.

Option 0, Modbus/PakBus, configures the datalogger to respond to Modbus RTU delivered via PakBus Datagrams with a destination Application ID of 502. A specific COM port is not specified as the Modbus request can be delivered over any active PakBus interface. This option is most commonly used in conjunction with an NL100 or with another logger configured with the DataGram() instruction. Additionally, this option is used when a user needs to route Modbus communications across a complex network or wants to use a single communications link for both Modbus and BMP5 communications via PakBus.

Setting the ComPort parameter to 502 or greater will specify the Modbus TCP/IP service port. The datalogger listens for Modbus TCP/IP communication on one IP port at a time; i.e., the logger will not listen on ports 502 and 503 at the same time. Avoid using port 6785 as that is the default PakBus TCP service port number.

A variable can be used in the ComPort parameter for use with functions that return a communication port.(for example,[TCPOpen](../Instructions/tcpopen.md)). If the ComPort parameter is set to 502, the datalogger will listen for a TCP connection on port number 502 and service TCP Modbus commands over TCP/IP instead of one one of the physical ComPorts.

Type: Constant
