# SerialOpenFormat (Error Detection Format)

The Format parameter is used to specify the type of error detection to be used for the exchange of data.These format options apply to all ComPorts except COM310 and COMSDC ports, as the format is ignored for these ports due to their predefined behavior.

If you are using SerialOpen to control a **SDM-SIO1A, SDM-SIO4A, or SDM-SIO2R** module,click herefor details on SerialOpen format parameters that are specific for SDM-SIO devices.

**NOTE:** Typical RS-232 connections use logic 1 low. Typical TTL connections use logic 1 high.

**NOTE:** With RS-485 and RS-422 communications, logic level is ignored. You must select the parity, stop, and data bits. However, either logic level can be used.

**NOTE:** All formats use one start bit.

| Code | Description                                                                                                                         |
| ---- | ----------------------------------------------------------------------------------------------------------------------------------- | --------------------- | ---- | ----------- |
| 0    | Logic 1 low;No parity, one stop bit, 8 data bits; No error checking; PakBus communication can occur concurrently on the same port   |
| 1    | Logic 1 low;Odd parity, one stop bit, 8 data bits                                                                                   |
| 2    | Logic 1 low;Even parity, one stop bit, 8 data bits                                                                                  |
| 3    | Logic 1 low;Binary, no parity, one stop bit, 8 data bits                                                                            |
| 4    | Logic 1 low;PakBus protocol active, 2 stop bits, 8 data bits                                                                        |
| 5    | Logic 1 low;Odd parity, two stop bits, 8 data bits                                                                                  |
| 6    | Logic 1 low;Even parity, two stop bits, 8 data bits                                                                                 |
| 7    | Logic 1 low;Binary, no parity, two stop bits, 8 data bits                                                                           |
| 9    | Logic 1 low;Odd parity, one stop bit, 7 data bits                                                                                   |
| 10   | Logic 1 low;Even parity, one stop bit, 7 data bits                                                                                  |
| 11   | Logic 1 low;Binary, no parity, one stop bit, 7 data bits                                                                            |
| 13   | Logic 1 low;Odd parity, two stop bits, 7 data bits                                                                                  |
| 14   | Logic 1 low;Even parity, two stop bits, 7 data bits                                                                                 |
| 15   | Logic 1 low;Binary, no parity, two stop bits, 7 data bits                                                                           | **C Terminals Only ** | Code | Description |
| ---  | ---                                                                                                                                 |
| 16   | Logic 1 high; No parity, one stop bit, 8 data bits; No error checking; PakBus communication can occur concurrently on the same port |
| 17   | Logic 1 high; Odd parity, one stop bit, 8 data bits                                                                                 |
| 18   | Logic 1 high; Even parity, one stop bit, 8 data bits                                                                                |
| 19   | Logic 1 high; Binary, no parity, one stop bit, 8 data bits                                                                          |
| 20   | Logic 1 high; PakBus protocol active, 2 stop bits, 8 data bits                                                                      |
| 21   | Logic 1 high; Odd parity, two stop bits, 8 data bits                                                                                |
| 22   | Logic 1 high; Even parity, two stop bits, 8 data bits                                                                               |
| 23   | Logic 1 high; Binary, no parity, two stop bits, 8 data bits                                                                         |
| 25   | Logic 1 high; Odd parity, one stop bit, 7 data bits                                                                                 |
| 26   | Logic 1 high; Even parity, one stop bit, 7 data bits                                                                                |
| 27   | Logic 1 high; Binary, no parity, one stop bit, 7 data bits                                                                          |
| 29   | Logic 1 high; Odd parity, two stop bits, 7 data bits                                                                                |
| 30   | Logic 1 high; Even parity, two stop bits, 7 data bits                                                                               |
| 31   | Logic 1 high; Binary, no parity, two stop bits, 7 data bits                                                                         |

By default, the RS-232 is set up for PakBus communication (i.e., PakBus active), unless changed by SerialOpen. However, the controlports arenot. They must first be set up for PakBus communication using option 4.

** NOTE:**Formats 0 and 16 ignores ASCII character 0 (Null) and ASCII characters above 127. Formats 3 and 19 should be used in place of formats 0 and 16 respectively, when handling binary data. Formats 0 and 16 should be used only for ASCII communication. Non-ASCII characters will be filtered by the PakBus communications stack while searching for a PakBus frame.

When disabling PakBus communication on the RS232 port, an alternative method of connection should be used, such as the ComUSB port.

Type: Constant
