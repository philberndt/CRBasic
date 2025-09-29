# ModbusVariable (Modbus Variable)

Used to specify the variable array that is used as the source of data to send, or the variable array that is used as the destination for data received. This variable can be formatted for Coil, Floating Point, or Integer Modbus registers and can be formatted as either Floating Point or Integer. If Variable is declared as Long or Boolean, then this parameter is set as a Modbus integer; otherwise, floating point Modbus will be used. Longs are assumed to be signed integers (numbers will be in the range -32768..+32767). The datalogger does not differentiate between holding and input registers. The only difference is the address offset. The specified array will be used for requests of input (address offset of 30000) or holding (address offset of 40000) registers.

- If this parameter is declared asBoolean

  **Note:** Data type used to represent conditions or hardware that have only two states (true or false) such as flags and control ports.

  and a Coil function code of 1, 2, 5, or 15 is used, it will be mapped to a Modbus coil in the other device.

- If this parameter is declared asFloat

  **Note:** Four-byte floating-point data type. Default datalogger data type for Public or Dim variables. Same format as IEEE4.

  and a Register function code of 3, 4, or 16 is used, it will be mapped to a Modbus Holding or Input register as floating point.

- If this parameter is declared asLong

  **Note:** Data type used when declaring a variable as an integer.

  and a Register function code of 3, 4, or 16 is used, it will be mapped to Modbus Holding or Input registers as an integer. Longs are assumed to be signed integers (numbers will be in the range -32768..+32767).

If option 1 is used for ModbusOption, Variable should be declared as a long. If option 05, 06, 15, or 16 is used for Function, this parameter can be a constant.

Floating point variables take two Modbus registers. The Modbus input registers are offset by 30000; Modbus holding registers are offset by 40000. Therefore, the first register corresponding to any array location X is holding register 40000+2X-1. For example, to retrieve array value number 3, you would ask for two registers starting with 40005.

If the ModbusServer is queried by the client using an odd number of registers, then the ModbusServer datalogger assumes the request is for a 16 bit integer.

Type: Variable
