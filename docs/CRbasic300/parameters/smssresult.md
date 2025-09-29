# ResultCode

A variable of Type Long or Float that holds the result of the instruction.

| Result Code | Description                                                                                                    |
| ----------- | -------------------------------------------------------------------------------------------------------------- |
| 0           | Timed out waiting for a response from the modem.                                                               |
| -1          | Message successfully sent to the network.                                                                      |
| -3          | Error response from modem.                                                                                     |
| -5          | PhoneNumber or Message parameter is an empty string.                                                           |
| -6          | Message sent to the modem, no confirmation response received from the modem.                                   |
| -7          | Timed out waiting for response from the modem.                                                                 |
| -8          | No internal cellular modem installed or modem is off/disabled                                                  |
| -9          | Datalogger timed out waiting for modem to send the message. Message is in Cell2XX queue and may still be sent. |
| -11         | Send failure. External Cell2XX is powered down                                                                 |
| -12         | Array out of bounds                                                                                            |

Type: Long or Float Variable or Variable Array
