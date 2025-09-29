# Function

Used to define what data is being sent to or received from the ModbusServer.

| Code | Name                       | Description                                                                                     |
| ---- | -------------------------- | ----------------------------------------------------------------------------------------------- |
| 01   | Read Coil/Port Status      | Reads the On/Off status of discrete output(s) in the ModbusServer                               |
| 02   | Read Input Status          | Reads the On/Off status of discrete input(s) in the ModbusServer                                |
| 03   | Read Holding Registers     | Reads the binary contents of holding register(s) in the ModbusServer                            |
| 04   | Read Input Registers       | Reads the binary contents of input register(s) in the ModbusServer                              |
| 05   | Force Single Coil/Por      | Forces a single Coil/Port in the ModbusServer to either On or Off                               |
| 06   | Write Single Register      | Writes a single register value (16-bit long) to a ModbusServer. (ModbusOption must be set to 1) |
| 15   | Force Multiple Coils/Ports | Forces multiple Coils/Ports in the ModbusServer to either On or Off                             |
| 16   | Write Multiple Registers   | Writes values into a series of holding registers in the ModbusServer                            |

Type: Variable or Constant
