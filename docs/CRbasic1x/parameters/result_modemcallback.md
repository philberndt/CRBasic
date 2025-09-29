# Result

The Result parameter is the variable in which a response code for the instruction will be stored. This response indicates the status of the dialing attempt (codes -30 through -35) and ultimately, whether or not the attempt was successful. A positive value indicates that there was no response to the request from the remote, and thus, the attempt was unsuccessful. A negative value indicates some other type of error occurred. A zero indicates a successful transaction. Because the support software will set the Abort variable to True to hang up the call, a -33 indicates that the callback attempt was successful or was aborted by the program or user in some other way. It will remain -33 until the Abort variable is set False by the program or some other means. The codes that can be returned are:

| Code    | Description                                                                                                                                                                                   |
| ------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 0       | Successful.                                                                                                                                                                                   |
| 1, 2..n | The number of timeouts waiting for a response. The value will increment with each successive failure. After a 0 or negative response, the value will start over at 1.                         |
| -1      | Response received but permission denied.                                                                                                                                                      |
| -16     | Table name and/or field name not present in the source datalogger, or the field is read only in the destination datalogger.                                                                   |
| -17     | Data type not supported.                                                                                                                                                                      |
| -18     | Array in the source datalogger is not dimensioned large enough to accommodate the values to be sent or array in the destination datalogger is not large enough to accommodate values received |
| -20     | Out of Comms memory.                                                                                                                                                                          |
| -21     | Failed to route packet when routing is set to auto-discover and route is not yet known.                                                                                                       |
| -22     | Communication port buffer exceeded.                                                                                                                                                           |
| -27     | DialSequence/EndDialSequence returned False so communication did not occur.                                                                                                                   |
| -30     | Dialing                                                                                                                                                                                       |
| -31     | Dialed successfully, waiting for connection                                                                                                                                                   |
| -32     | Connected, sending message to LoggerNet                                                                                                                                                       |
| -33     | Attempt aborted by abort expression                                                                                                                                                           |
| -34     | ComPort is busy with some other function                                                                                                                                                      |
| -35     | Modem not echoing AT commands, sending +++ and toggling power to modem                                                                                                                        |

Type: Variable
