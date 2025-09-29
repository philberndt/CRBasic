# Option

A bit field that is used to configure the operation of the SPI interface. The bits are defined as:

| Field                   | Description                                                                                                                                                 |
| ----------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 7 Clock Phase (CPHA)    | 0 = Data is changed on the first CLK edge and captured on the following edge. 1 = Data is captured on the first CLK edge and changed on the following edge. |
| 6 Clock Polarity (CPOL) | 0 = The inactive state is low; 1 = The inactive state is high.                                                                                              |
| 5 Data Order            | 0 = LSB first; 1 = MSB first                                                                                                                                |
| 4 Byte length           | 0 = 8 bit and 1 =7 bit                                                                                                                                      |

For example: Option field of &H60 Phase = 0, Polarity = 1, Data = 1 and Length = 0

Or Option field of &H30 Phase = 0, Polarity = 0, Data = 1 and Length = 1

The Diagram below demonstrates different SPI interface configurations and may be used to match the configuration needed for your specific sensor:

Source:<https://commons.wikimedia.org/wiki/File:SPI_timing_diagram.svg>

Type: Constant
