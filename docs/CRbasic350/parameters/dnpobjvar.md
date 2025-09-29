# DNPObject (DNP Object Number)

Specifies the DNP3 object number to be used.

# DNPVariation (DNP Variation)

Specifies the DNP3 variation to be used.

The following objects and variations are supported:

| Object | Variation | Description                                             |
| ------ | --------- | ------------------------------------------------------- |
| 1      | 1         | Static, Binary Input                                    |
| 1      | 2         | Static, Binary Input with Status                        |
| 20     | 1         | Counter, 32-bit with flag                               |
| 20     | 2         | Counter, 16-bit with flag                               |
| 20     | 3         | Counter, 32-bit with flag, delta                        |
| 20     | 5         | Counter, 32-bit without flag                            |
| 20     | 6         | Counter, 16-bit without flag                            |
| 30     | 1         | Static, 32-Bit Analog Input                             |
| 30     | 2         | Static,16-Bit Analog Input                              |
| 30     | 3         | Static, 32-Bit Analog Input without Flag                |
| 30     | 4         | Static, 16-Bit Analog Input without Flag                |
| 30     | 5         | Analog input, single precision floating point with flag |
| 32     | 1         | Event, 32-Bit Analog Change Event without Time          |
| 32     | 2         | Event, 16-Bit Analog Change Event without Time          |
| 32     | 3         | Event, 32-Bit Analog Change Event with Time             |
| 32     | 4         | Event, 16-Bit Analog Change Event with Time             |
