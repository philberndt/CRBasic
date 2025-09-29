# ResultCode (Result Code)

The variable in which a response code for the transmission will be stored. A zero indicates a successful transaction. A positive value indicates that there was no response to the request from the remote. A negative value indicates some other type of error occurred. The codes that can be returned are:

| Code    | Description                                                                                                                                                                                   |
| ------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 0       | Successful.                                                                                                                                                                                   |
| -1      | Response received but permission denied.                                                                                                                                                      |
| -2      | The Master datalogger cannot find this destination device in its Network instructions list.                                                                                                   |
| -16     | Table name and/or field name not present in the source datalogger, or the field is read only in the destination datalogger.                                                                   |
| -18     | Array in the source datalogger is not dimensioned large enough to accommodate the values to be sent or array in the destination datalogger is not large enough to accommodate values received |
| -20     | Out of Comms memory.                                                                                                                                                                          |
| -21     | Failed to route packet when routing is set to auto-discover and route is not yet known.                                                                                                       |
| -22     | Communication port buffer exceeded.                                                                                                                                                           |
| -27     | DialSequence/EndDialSequence returned False so communication did not occur.                                                                                                                   |
| 1, 2..n | The number of timeouts waiting for a response. The value will increment with each successive failure. After a 0 or negative response, the value will start over at 1.                         |

Type: Variable
