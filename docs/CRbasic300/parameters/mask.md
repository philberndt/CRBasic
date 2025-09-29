# Mask

The Mask allows the read or write to only act on certain ports. It is a binary representation of the ports as follows:

|         |      |       |      |      |      |      |     |     |
| ------- | ---- | ----- | ---- | ---- | ---- | ---- | --- | --- |
| Bit No. | 8    | 7     | 6    | 5    | 4    | 3    | 2   | 1   |
| Port    | P_SW | SW12V | SE4  | SE3  | SE2  | SE1  | C2  | C1  |
| High V  | ~3.3 | ~12\* | ~3.3 | ~3.3 | ~3.3 | ~3.3 | ~5  | ~5  |
| Low V   | 0    | 0     | 0    | 0    | 0    | 0    | 0   | 0   |

\* Voltage on a SW12 terminal will change with datalogger supply voltage.

**ReadIO **: If a port position is set to 1, the datalogger reads the status of the port. If a port position is set to 0 the datalogger ignores the status of the port (the Mask is "anded" with the port status; the "and" operation returns a 1 for a digit if the Mask digit and the port status are both 1, and a 0 if either or both is 0). CRBasic allows the entry of numbers in binary format by preceding the number with "&B". For example, if the Mask is entered as &B100 (leading zeros can be omitted in binary format just as in decimal) and ports SE1 and C1 are high, the result of the instruction will be 4 (decimal, binary = 100); if port SE1 is low, the result would be 0.

** WriteIO**: If a port position in the mask is set to 1, the datalogger sets the port based on the value for that port in the Source. If a port position in the mask is set to 0 the value in the Source is ignored. Binary numbers are entered into CRBasic by preceding the number with "&B". For example, if the mask is entered as &B110 (leading zeros can be omitted in binary format just as in decimal) and the source is 5 decimal (binary 101) SE1 will be set high and C2 will be set low. The mask indicates that only bits 3 and 2 should be set. While the value of the source also has a 1 for bit 1, it is ignored because the mask indicates 1 should not be changed.

Type: Binary value (preceded by &B) or Integer (from 0 to 255 representing the binary value)
