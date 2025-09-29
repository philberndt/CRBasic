# StatusCommand (Type of Information Requested)

Integer used to indicate the type of information requested from the transmitter. One of the following numeric codes are entered:

- Code **0**=[Read Time](#Code)

- Code ** 1**=[Status](#Code2)

- Code ** 2**=[Last Message Status](#Code3)

- Code ** 3**=[Transmit Random Message](#Code4)

- Code ** 4**=[Read Error Register](#Code5)

- Code ** 5**=[Reset Error Register](#Code6)

- Code ** 6**=[Return Transmitter to Online Mode](#Code7)

The codes then return a response of:

| Command Code Result | Description                                                                                |
| ------------------- | ------------------------------------------------------------------------------------------ |
| 0                   | Command executed successfully                                                              |
| 1                   | Checksum error in response string                                                          |
| 2                   | Timeout waiting for response after addressing                                              |
| 3                   | Something besides expected response received after addressing                              |
| 4                   | Received improper acknowledgment from transmitter                                          |
| 5                   | Timed out while waiting for an acknowledgment from transmitter                             |
| 6                   | CS I/O port unavailable; GOES not attached                                                 |
| 7                   | Transmit random message failure; possibly no data in random buffer, enable random messages |
| 9                   | Invalid command code                                                                       |

### Code 0 = Read Time

This option requires that the ResultCode array be dimensioned to 4. Results returned are:

- (1) Command Code Result

- (2) Hours

- (3) Minutes

- (4) Seconds (time reflected in GMT)

For returned response codes, see [Command Code Results](#returnresponse).

### Code 1 = Status

This option requires that the ResultCode array be dimensioned to 13. Results returned are:

- (1) Command Code Result

- (2) Number of bytes in self-timed buffer

- (3) Days to next self-timed message

- (4) Hours to next self-timed message

- (5) Minutes to next self-timed message

- (6) Seconds to next self-timed message

- (7) Number of bytes in random buffer

- (8) Hours to next random message

- (9) Minutes to next random message

- (10) Seconds to next random message

- (11) Fail safe position (1 indicates transmitter disabled due to fail-safe)

- (12) Power supply (in tenths of volts, under 1A load)

- (13) Average GPS acquisition time (tens of seconds)

For returned response codes, see [Command Code Results](#returnresponse).

### Code 2 = Last Message Status

This option requires that the ResultCode array be dimensioned to 14. Results returned are:

- (1) Command Code Result

- (2) Message type (self-timed or random)

- (3) Number of bytes in message

- (4) Forward RF power (tenths of Watts)

- (5) Reflected RF power (tenths of Watts)

- (6) Power supply (tenths of volts, under full load)

- (7) GPS acquisition time (tens of seconds)

- (8) Oscillator drift (signed, hundreds of Hz)

- (9) Latitude degrees

- (10) Latitude minutes

- (11) Latitude seconds

- (12) Longitude degrees

- (13) Longitude minutes

- (14) Longitude seconds

For returned response codes, see [Command Code Results](#returnresponse).

### Code 3 = Transmit Random Message

This option requires that the ResultCode array be dimensioned to 1. The only result returned is the Command Code Result.

### Code 4 = Read Error Register

This option requires that the ResultCode array be dimensioned to 10. Results returned are:

- (1) Command Code Result

- (2) Number of errors

- (3) Command

- (4) Error Code

- (5) Command

- (6) Error Code

- (7) Command

- (8) Error Code

- (9) Command

- (10) Error Code

Error Codes

| Hex  | Decimal | Description                                                                             |
| ---- | ------- | --------------------------------------------------------------------------------------- |
| 0x00 | 00      | No error                                                                                |
| 0x01 | 01      | Illegal command                                                                         |
| 0x02 | 02      | Command rejected                                                                        |
| 0x03 | 03      | Illegal checksum or too much data                                                       |
| 0x04 | 04      | Time out or too little data                                                             |
| 0x05 | 05      | Illegal parameter                                                                       |
| 0x06 | 06      | Transmit buffer overflow                                                                |
| 0x10 | 16      | Message abort due to PLL                                                                |
| 0x11 | 17      | Message abort due to GPS                                                                |
| 0x12 | 18      | Message abort due to power supply (battery voltage less than 9.6 V during transmission) |
| 0x13 | 19      | Software fault                                                                          |
| 0x14 | 20      | Fail-safe fault                                                                         |
| 0x15 | 21      | GPS time sync fault (GPS could not acquire fix)                                         |
| 0x16 | 22      | SWR fault -- transmission antenna connection                                            |
| 0x1F | 31      | Internal error with SAT HDR GOES                                                        |

Nine registers are used to store error information. Up to 255 errors are stored, along with the command that was issued when the error occurred and a code specific to the type of the error. Internal fault codes are also stored. The error is transmitted to the datalogger as a hexadecimal value; the datalogger converts this to a decimal value and stores it in the array.

For returned response codes, see [Command Code Results](#returnresponse).

### Code 5 = Reset Error Register

This option requires that the ResultCode array be dimensioned to 1. The only result returned is the Command Code Result.

### Code 6 = Return Transmitter to Online Mode

This option returns the SAT HDR GOES to an online mode. It is typically used after a forced random transmission (StatusCommand 3). This option requires that the ResultCode array be dimensioned to 1. The only result returned is the Command Code Result.
