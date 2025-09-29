# SDICommand (SDI-12 Command)

Specifies the command-string data to read from the other datalogger. The command should be enclosed in quotes. Multiple SDI12Watch instructions can be used to retrieve data from different SDI-12 commands.

| Command   | Description                                                    |
| --------- | -------------------------------------------------------------- |
| ?!        | Address query                                                  |
| An!       | Change address (where n = address)                             |
| C!        | Initiate concurrent measurements                               |
| CC!       | Initiate concurrent measurements (with checksum)               |
| M!        | Initiate measurements                                          |
| MC!       | Initiate measurements (with checksum)                          |
| M1! - M9! | Additional measurement commands specified by the SDI-12 sensor |
| RC!       | Continuous measurement (with checksum)                         |
| R0! - R9! | Continuous measurement commands                                |
| V!        | Initiate verify sequence                                       |
