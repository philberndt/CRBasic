# ResultCode (Result Code)

The ResultCode variable holds the results of the communication attempt. ResultCode is set to 0 if communication is successful; it increments by 1 after a communication failure (a failure is an expiration of the timeout period and any Tries). If the Modbus server returns a Modbus exception code, it will be preceded by a minus sign and stored in ResultCode. Error codes returned by the server:

| Code | Name                                                                                                                                                                                                        |
| ---- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| -01  | Illegal function: the function code received in the query is not an allowable action for the device. The device may not support the function or it may not be in a state to process the request.            |
| -02  | Illegal data address: the data address received in the query is not an allowable address for the device. The combination of the reference number and transfer length may be invalid.                        |
| -03  | Illegal data value: the value contained in the query data field is not an allowable value for the device.                                                                                                   |
| -04  | Server device failure: an unrecoverable error occurred while the device was attempting to perform the requested action.                                                                                     |
| -05  | Acknowledge: the device has accepted the request and is processing it, but a long duration of time will be required to do so. This is a specialized function used in conjunction with programming commands. |
| -06  | Server device busy: the device is engaged in processing a long-duration program command.                                                                                                                    |
| -08  | Memory parity error: the device attempted to read a record file but detected a parity error in the memory. Used in conjunction with function codes 20 and 21.                                               |
| -09  | Gateway path unavailable: indicates that the gateway was unable to allocate an internal communication path from the input port to the output port for processing the request.                               |
| -10  | ModbusClient error: received an unexpected function code response from server.                                                                                                                              |
| -11  | The specified ComPort (or TCP socket) is not opened. There is no connection to the Modbus server.                                                                                                           |
| -16  | ModbusClient error: out of comms memory.                                                                                                                                                                    |
| -20  | ModbusClient error: variable not dimensioned large enough to store results from server.                                                                                                                     |

Type: Variable
