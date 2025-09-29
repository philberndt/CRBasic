# SIO4Cmd

Used to configure the SIO4. The commands are listed briefly below. See the SDM-SIO4 manual for details.

| Code | Description                                                                              |
| ---- | ---------------------------------------------------------------------------------------- |
| 1    | Poll of available data                                                                   |
| 2    | Get EPROM and memory signatures.                                                         |
| 3    | Flush all receive buffers.                                                               |
| 4    | Send data to datalogger.                                                                 |
| 5    | Return number of watchdog errors, invalid command executed, and lithium battery voltage. |
| 6    | Flush transmit buffer.                                                                   |
| 7    | Activate command line.                                                                   |
| 8    | Poll TX buffers for data.                                                                |
| 9    | Flush converted data buffer.                                                             |
| 66   | Send single-byte data to datalogger.                                                     |
| 67   | Get return code                                                                          |
| 320  | Send byte data to SDM-SIO4.                                                              |
| 321  | Execute command line command.                                                            |
| 1024 | Send string to SIO4.                                                                     |
| 1025 | Transmit a byte.                                                                         |
| 1026 | Serial port status.                                                                      |
| 1027 | Manual handshake mode.                                                                   |
| 2049 | Communication parameters.                                                                |
| 2054 | Set up receive filter.                                                                   |
| 2304 | Transmit string and/or data to device (formatter/filter).                                |
| 2305 | Transmit bytes.                                                                          |
