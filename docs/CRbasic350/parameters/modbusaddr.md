# ModbusAddr (Modbus Address)

The Modbus address of the server. Valid range is 1-255 (excluding 189, which is the PakBus sync byte). Address 0 is a broadcast address. Multiple addresses can be assigned to a single interface by using multiple ModbusClient() instructions

The ModbusAddr can be used to specify an offset for the starting register. Enter Starting Register \* 1000 + the address. (i.e., the most significant digits specify the offset and the last three digits specify the Modbus address). For instance, for a starting register of 1500 and an address of 3, enter 1500003.

**NOTE:** If ModbusAddr is set to a negative value, the datalogger will discard any bytes that are echoed back to it.

Type: Variable or Integer
