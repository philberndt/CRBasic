# Length

Specifies the number of CRBasic variables to act upon with this instruction. When reading coils or when using a 16-bit related ModbusOption parameter of 1, 3, 11, or 13, the Length parameter will match the number of 16 bit register values requested or acted upon. When using a 32-bit related ModbusOption parameter of 0,2,10, or 12, the datalogger will automatically double the Length value specified so that it requests 32-bits of data for each CRBasic variable.

Type: Variable of Constant
