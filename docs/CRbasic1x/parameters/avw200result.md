# Result

Stores the success or failure of the datalogger's communication attempt with the AVW200. The result codes are as follows:

| Code | Description                                                                                                                  |
| ---- | ---------------------------------------------------------------------------------------------------------------------------- |
| 0    | Communication successful. Values have been written to the destination array.                                                 |
| ≥1   | Number of communication failures. Old values will remain in the destination array. Resets to 0 upon successful communication |
| -3   | First communication. Values will be available on the next scan.                                                              |
| -5   | Request for another measurement received when the AVW200 is busy making a measurement.                                       |
| -20  | Out of Comms memory                                                                                                          |

Type: Variable
