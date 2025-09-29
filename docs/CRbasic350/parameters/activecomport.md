# ComPort (Communications Port)

The communications port that will be used by the instruction. Right-click to display a list. Options vary depending on the instruction.

| Alphanumeric | Description                                   |
| ------------ | --------------------------------------------- |
| ComUSB       | USB port of the datalogger                    |
| ComRS232     | RS232 port of the datalogger                  |
| Com1         | Datalogger control terminals C1(TX) & C2 (RX) |
| Com2         | COM2 (TX,RX) of the datalogger                |
| Com3         | COM3 (TX,RX) of the datalogger                |
| ComRF        | Integrated RF communication                   |

If the instruction has a ResultCode parameter and a negative value is entered for the ComPort, the datalogger will not wait on a response from the destination device before proceeding to the next instruction.

Type: Constant. For all instructions except SerialOpen, this parameter can also be a variable.
