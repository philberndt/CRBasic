# Source

A variable or variable array dimensioned as a Long that holds the integer value that will be sent to the SDM-CD16AC. The SDM-CD16AC ports will be enabled/disabled following the bit pattern of the Source variable. For example, if the decimal value 21845 (hex &h5555 or binary &b0101010101010101) is sent to the SDM-CD16AC using the SDMCD16Mask () instruction, all the odd ports will be enabled.

Type: Long
