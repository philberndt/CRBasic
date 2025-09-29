# ComPort_Serial_In (Communications Port Serial In)

The COM port that will be used by the instruction.Note thatbeginning with OS 6.0,control ports C1 and C2 can be configured individually for transmit (Tx) or receive (Rx).Right-click to display a list.

| Alphanumeric | Description                                |
| ------------ | ------------------------------------------ |
| ComUSB       | USB port of the datalogger                 |
| ComRS232     | RS232 port of the datalogger               |
| Com1         | Datalogger's control ports 1 (TX) & 2 (RX) |
| ComC1_Rx     | Configures control port 1 for receive only |
| ComC2_Rx     | Configures control port 2 for receive only |
| ComRF        | Integrated RF communication                |

Type: Constant. For all instructions except SerialOpen, this parameter can also be a variable. However, if a variable is used, the port must be opened previously by [SerialOpen (Open Communication Port)](../Instructions/serialopen.md)
