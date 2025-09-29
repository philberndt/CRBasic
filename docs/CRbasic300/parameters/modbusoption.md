# ModbusOption (Modbus Option)

An optional parameter that is used to specify the data type of the Modbus variables.

| Code        | Description                                                                                                     |
| ----------- | --------------------------------------------------------------------------------------------------------------- |
| 0 or absent | Default; 32-bit float or Long, the standard ordering of the 2 byte registers are reversed (CDAB).               |
| 1           | If the Modbus variable array is defined as a Long, variables are treated as 16-bit signed integers.             |
| 2           | If the Modbus variable array is defined as a 32-bit float or a Long, with no reversal of the byte order (ABCD). |
| 3           | If the Modbus variable array is defined as a Long, variables are treated as 16-bit unsigned integers.           |
| 10          | Modbus ASCII, 4 bytes, CDAB.                                                                                    |
| 11          | Modbus ASCII, 2 bytes, signed.                                                                                  |
| 12          | Modbus ASCII, 4 bytes, ABCD.                                                                                    |
| 13          | Modbus ASCII, 2 bytes, unsigned.                                                                                |

Type: Variable
