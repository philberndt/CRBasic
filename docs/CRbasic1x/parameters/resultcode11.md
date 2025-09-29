# ResultCode (Instruction Result)

The variable in which a response code for the transmission will be stored. ResultCode will return:

| Code | Description                                                            |
| ---- | ---------------------------------------------------------------------- |
| 0    | Success.                                                               |
| 1    | The module did not return success.                                     |
| 2    | Unable to open the file.                                               |
| 3    | Module not identified on the bus.                                      |
| 4    | CAN send failure.                                                      |
| 5    | Timeout waiting for a response from the module after sending an image. |

Type: Variable (Float)
