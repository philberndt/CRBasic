# DNPVariation (DNP Variation)

Assigns the Source parameter to a specific variation, which describes the data format within its Object group. Datalogger supported DNP objects and variations:

**NOTE:** Object 0 device attribute variations are read only and cannot be specified in the DNPVariable instruction. Rather, they are sent only when requested by a master device.

| Object | Variation | Description                                                                        |
| ------ | --------- | ---------------------------------------------------------------------------------- |
| 0      | 240       | Device attribute, maximum transmit fragment size                                   |
| 0      | 241       | Device attribute, maximum receive fragment size                                    |
| 0      | 242       | Device attribute, OS version                                                       |
| 0      | 243       | Device attribute, rev board, TI chip                                               |
| 0      | 247       | Device attribute, station name                                                     |
| 0      | 248       | Device attribute, serial number                                                    |
| 0      | 250       | Device attribute, product name and model                                           |
| 0      | 252       | Device attribute, name of device manufacturer                                      |
| 0      | 254       | Device attribute, return all devices attributes in single response                 |
| 0      | 255       | Device attribute, retrieve all of the supported device attribute variation numbers |
| 1      | 1         | Static, Binary Input                                                               |
| 1      | 2         | Static, Binary Input with Status                                                   |
| 2      | 1         | Event, Binary Input Change without Time                                            |
| 2      | 2         | Event, Binary Input Change with Time                                               |
| 2      | 3         | Event, Binary Input Change with Relative Time                                      |
| 10     | 1         | Static, Binary Output, Packed Format                                               |
| 10     | 2         | Static, Binary Output Status, with Flags                                           |
| 12     | 1         | Binary command, control relay output block (CROB)                                  |
| 20     | 1         | Counter, 32-bit with flag                                                          |
| 20     | 2         | Counter, 16-bit with flag                                                          |
| 20     | 3         | Counter, 32-bit with flag, delta                                                   |
| 20     | 4         | Counter, 16-bit with flag, delta                                                   |
| 20     | 5         | Counter, 32-bit without flag                                                       |
| 20     | 6         | Counter, 16-bit without flag                                                       |
| 20     | 7         | Counter, 32-bit without flag, delta                                                |
| 20     | 8         | Counter, 16-bit without flag, delta                                                |
| 21     | 1         | Frozen counter, 32-bit with flag                                                   |
| 21     | 2         | Frozen counter, 16-bit with flag                                                   |
| 21     | 5         | Frozen counter, 32-bit with flag and time                                          |
| 21     | 6         | Frozen counter, 16-bit with flag and time                                          |
| 21     | 9         | Frozen counter, 32-bit without flag                                                |
| 21     | 10        | Frozen counter, 16-bit without flag                                                |
| 22     | 1         | Counter event, 32-bit with flag                                                    |
| 22     | 2         | Counter event, 16-bit with flag                                                    |
| 22     | 5         | Counter event, 32-bit with flag and time                                           |
| 22     | 6         | Counter event, 16-bit with flag and time                                           |
| 30     | 1         | Static, 32-Bit Analog Input                                                        |
| 30     | 2         | Static,16-Bit Analog Input                                                         |
| 30     | 3         | Static, 32-Bit Analog Input without Flag                                           |
| 30     | 4         | Static, 16-Bit Analog Input without Flag                                           |
| 30     | 5         | Analog input, single precision floating point with flag                            |
| 32     | 1         | Event, 32-Bit Analog Change Event without Time                                     |
| 32     | 2         | Event, 16-Bit Analog Change Event without Time                                     |
| 32     | 3         | Event, 32-Bit Analog Change Event with Time                                        |
| 32     | 4         | Event, 16-Bit Analog Change Event with Time                                        |
| 32     | 5         | Analog input event, single precision floating point without time                   |
| 32     | 7         | Analog input event, single precision floating point with time                      |
| 40     | 1         | Analog output status, 32-bit with flag                                             |
| 40     | 2         | Analog output status, 16-bit with flag                                             |
| 40     | 3         | Analog output status, single precision floating point with flag                    |
| 41     | 1         | Analog output, 32-bit                                                              |
| 41     | 2         | Analog output, 16-bit                                                              |
| 41     | 3         | Analog output, single precision floating point                                     |
| 50     | 1         | Read, Time and Date                                                                |
| 60     | 2         | Class 1 Data                                                                       |
| 60     | 3         | Class 2 Data                                                                       |
| 60     | 4         | Class 3 Data                                                                       |
| 110    | X         | Octet string; variation parameter is used to declare maximum length of string      |

Type: Constant
