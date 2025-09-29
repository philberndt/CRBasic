# ComPort (Communication Port)

The ComPort parameter specifies the communication port that will be used for the exchange of data.

| Alphanumeric | Description                                |
| ------------ | ------------------------------------------ |
| ComUSB       | USB port of the datalogger                 |
| ComRS232     | RS232 port of the datalogger               |
| Com1         | Datalogger's control ports 1 (TX) & 2 (RX) |
| ComRF        | Integrated RF communication                |

**DNP over TCP** To send DNP master commands over TCP, a socket to the outstation can be opened via the [TCPOpen()](../Instructions/tcpopen.md) instruction. The port number specified must match between the DNP master and the DNP outsation in order for the DNP master to make a connection with the outstation. The default port number for DNP over TCP in a Campbell Scientific datalogger configured as a DNP outstation is 20000.
