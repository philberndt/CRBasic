# Bridge

A 2-by-3 variable array of type Long that holds the comport, received bytes, and sent bytes for each of the two ports in the bridge. Zeros are used as place holders for received and sent bytes until bytes are actually received or sent. If an error occurs sending bytes, sent bytes will increment negatively for each failure to send.

If a comport is a TCP connection handle, 0 is used as a place holder for the comport until a successful TCP connection is made. For example, Public Bridge1(2,3) As Long = {COMC1,0,0,0,0,0}. In this example, COMC1 is the first comport and a TCP connection handle is the second comport. When a successful TCP connection is made, the 0 for the second comport will be replaced with a socket/connection number.

Valid Comports include:

| Alphanumeric | Description                                 |
| ------------ | ------------------------------------------- |
| ComUSB       | USB port of the datalogger                  |
| ComRS232     | RS232 port of the datalogger                |
| Com1         | Datalogger's control ports 1 (TX) & 2 (RX)  |
| Com2         | COM2 (TX & RX) port of the datalogger       |
| Com3         | COM3 (TX & RX) port of the datalogger       |
| ComC1_TX     | Configures control port 1 for transmit only |
| ComC1_RX     | Configures control port 1 for receive only  |
| ComC2_TX     | Configures control port 2 for transmit only |
| ComC2_RX     | Configures control port 2 for receive only  |
| ComRF        | Integrated RF communication                 |

ComPort can also be a virtual ComPort, such as the result of a TCPOpen instruction.

Type: Variable as 2-by-3 array of type Long
