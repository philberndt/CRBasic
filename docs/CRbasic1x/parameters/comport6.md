# ComPort (Communication Port)

The ComPort parameter specifies the communication port that will be used for outputting data. When communicating over TCP/IP, use the variable returned by the TCPOpen function as the ComPort for GOESTable. If the PakBusTCPServer setting is being used rather than TCPOpen, use the Route instruction for the ComPort parameter.

| Alphanumeric | Description                        |
| ------------ | ---------------------------------- |
| ComRS232     | RS232 port of the datalogger       |
| ComC1        | Datalogger control terminals 1 & 2 |
| ComC3        | Datalogger control terminals 3 & 4 |
| ComC5        | Datalogger control terminals 5 & 6 |
| ComC7        | Datalogger control terminals 7 & 8 |
| COMSDC7      | Datalogger CS I/O port; SDC7       |
| COMSDC8      | Datalogger CS I/O port; SDC8       |
| COMSDC10     | Datalogger CS I/O port; SDC10      |
| COMSDC11     | Datalogger CS I/O port; SDC11      |

**NOTE:** When using an SDC comport, an SC105 with a female-to-female null modem cable should be used. The SC105 baud rates for both the CS I/O and RS-232 ports should be set to 9600 baud.

Type: Constant or Variable
