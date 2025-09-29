# Bridge

A 2-by-3 variable array of type Long that holds the comport, received bytes, and sent bytes for each of the two ports in the bridge. Zeros are used as place holders for received and sent bytes until bytes are actually received or sent. If an error occurs sending bytes, sent bytes will increment negatively for each failure to send.

If a comport is a TCP connection handle, 0 is used as a place holder for the comport until a successful TCP connection is made. For example, Public Bridge1(2,3) As Long = {COMC1,0,0,0,0,0}. In this example, COMC1 is the first comport and a TCP connection handle is the second comport. When a successful TCP connection is made, the 0 for the second comport will be replaced with a socket/connection number.

Valid Comports include:

| Alphanumeric | Description                           |
| ------------ | ------------------------------------- |
| ComRS232     | RS232 port of the datalogger          |
| ComME        | Datalogger CS I/O port; modem enabled |
| Com310       | Datalogger CS I/O port; COM310 modem  |
| Com320       | Datalogger CS I/O port, COM320 modem  |
| ComSDC7      | Datalogger CS I/O port; SDC7          |
| ComSDC8      | Datalogger CS I/O port; SDC8          |
| ComSDC10     | Datalogger CS I/O port; SDC10         |
| ComSDC11     | Datalogger CS I/O port; SDC11         |
| ComC1        | Datalogger control terminals 1 & 2    |
| ComC3        | Datalogger control terminals 3 & 4    |
| ComC5        | Datalogger control terminals 5 & 6    |
| ComC7        | Datalogger control terminals 7 & 8    |

ComPort can also be a virtual ComPort, such as the result of a TCPOpen instruction.

Beginning withCR1000X OS 05, an SDM-SIO1A or SDM-SIO4A may be used as a ComPort. See the [SerialOpen (Open Communication Port)](../Instructions/serialopen.md) help for valid SDM-SIOxA port numbers.

Type: Variable as 2-by-3 array of type Long
