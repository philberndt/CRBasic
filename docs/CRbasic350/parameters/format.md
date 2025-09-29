# SerialOpenFormat (Error Detection Format)

The Format parameter is used to specify the type of error detection to be used for the exchange of datausing RS-232 logic.

**NOTE:** All formats use one start bit.

| Code | Description                                                                                                                       |
| ---- | --------------------------------------------------------------------------------------------------------------------------------- |
| 0    | Logic 1 low;No parity, one stop bit, 8 data bits; No error checking; PakBus communication can occur concurrently on the same port |
| 1    | Logic 1 low;Odd parity, one stop bit, 8 data bits                                                                                 |
| 2    | Logic 1 low;Even parity, one stop bit, 8 data bits                                                                                |
| 3    | Logic 1 low;Binary, no parity, one stop bit, 8 data bits                                                                          |
| 4    | Logic 1 low;PakBus protocol active, 2 stop bits, 8 data bits                                                                      |
| 5    | Logic 1 low;Odd parity, two stop bits, 8 data bits                                                                                |
| 6    | Logic 1 low;Even parity, two stop bits, 8 data bits                                                                               |
| 7    | Logic 1 low;Binary, no parity, two stop bits, 8 data bits                                                                         |
| 9    | Logic 1 low;Odd parity, one stop bit, 7 data bits                                                                                 |
| 10   | Logic 1 low;Even parity, one stop bit, 7 data bits                                                                                |
| 11   | Logic 1 low;Binary, no parity, one stop bit, 7 data bits                                                                          |
| 13   | Logic 1 low;Odd parity, two stop bits, 7 data bits                                                                                |
| 14   | Logic 1 low;Even parity, two stop bits, 7 data bits                                                                               |
| 15   | Logic 1 low;Binary, no parity, two stop bits, 7 data bits                                                                         |

By default, the RS-232 is set up for PakBus communication (i.e., PakBus active), unless changed by SerialOpen. However, the controlport isnot. They must first be set up for PakBus communication using option 4.

**NOTE:** Format 0 ignores ASCII character 0 (Null) and ASCII characters above 127. Format 3 should be used in place of format 0 when handling binary data. Format 0 should be used only for ASCII communication. Non-ASCII characters will be filtered by the PakBus communications stack while searching for a PakBus frame.

When disabling PakBus communication on the RS232 port, an alternative method of connection should be used, such as the ComUSB port.

Type: Constant
