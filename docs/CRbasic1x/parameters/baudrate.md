# BaudRate (Data Transmit Rate)

The rate, in bps, at which data is transmitted. The options are 300,600,1200, 2400, 4800, 9600, 19200, 38400, 57600, and 115200. Selecting one of these options fixes the baud rate at that rate of communication.If a negative baud rate is entered, the first communication attempt will be at the specified baud rate, but if communication fails at that rate, the datalogger will go into autobaud mode (where it will try different rates until successful or until the instruction times out).

**NOTE:** Autobaud is not available on control ports used as com ports. Baud rate for SDC ports must be 9600 or greater.If a serial port is opened, it must be closed before changing the port baud rate.

**NOTE:** If you are using SerialOpen to control a SDM-SIOx (SDM-SI01A, SDM-SIO1A, SDM-SIO2R) automatic baud rate detection is not supported. Rather, setting the baud rate to a negative value enables automatic flow control (RTS/CTS).Click herefor additional information.

Right-click this parameter to display a list.

**Type:** Constant. In [SerialOpen](../Instructions/serialopen.md), BaudRate can be a variable.
