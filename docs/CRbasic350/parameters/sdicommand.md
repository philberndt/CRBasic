# SDICommand (SDI Command)

Used to specify the command strings that will be sent to the sensor. The command must be enclosed in quotes.

| Command   | Description                                                              |
| --------- | ------------------------------------------------------------------------ |
| ?!        | Address query                                                            |
| aAb!      | Change address (where a = current address and b = new address)           |
| HB!       | High-volume binary data                                                  |
| HA!       | High-volume ASCII data                                                   |
| C!        | Initiate concurrent measurements                                         |
| CC!       | Initiate concurrent measurements (with checksum)                         |
| I!        | Send identification (destination variable must be formatted as a string) |
| M!        | Initiate measurements                                                    |
| MC!       | Initiate measurements (with checksum)                                    |
| M1! - M9! | Additional measurement commands specified by the SDI-12 sensor           |
| RC!       | Continuous measurement (with checksum)                                   |
| R0! - R9! | Continuous measurement commands                                          |
| V!        | Initiate verify sequence                                                 |
| X!        | Extended commands (destination variable must be formatted as a string)   |

If a check summed command fails, a NAN will be returned and the command will be retried.

Type: Constant or variable
