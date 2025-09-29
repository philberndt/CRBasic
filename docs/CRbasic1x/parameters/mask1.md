# Mask

Used to select which ports will be affected by this instruction. It is a binary representation of the portsas follows:

|         |     |     |     |     |     |     |     |     |
| ------- | --- | --- | --- | --- | --- | --- | --- | --- |
| Bit No. | 8   | 7   | 6   | 5   | 4   | 3   | 2   | 1   |
| Port    | C8  | C7  | C6  | C5  | C4  | C3  | C2  | C1  |

If a port position is set to 1, the datalogger configures that port. Binary numbers are entered by preceding the number with "&B". Leading zeros can be omitted. As an example, if &B110 is entered for this parameter, ports at bit no 3 and 2 will be configured, based on the Function parameter.

Type: Binary value (preceded by &B) or Integer (from 0 to 255 representing the binary value)
