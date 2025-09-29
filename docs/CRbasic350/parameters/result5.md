# Result

Specifies the public variable that holds the result code of the communication attempt. The result code is 0 if the communication is successful. The result code increments by 1 with each communication failure. If a response is received from the DNP3 outstation but there is a communication error, one of the following error codes is returned.

| Code | Description         |
| ---- | ------------------- |
| -1   | Out of Bounds Error |
| -2   | Data CRC Error      |
| -3   | Write Error         |
| -4   | Fragment Error      |
| -5   | OverFlow Error      |
| -6   | Function Error      |
| -7   | Group/Object Error  |
| -8   | Variation Error     |
| -9   | Qualifier Error     |
| -10  | Quantity Error      |
| -11  | ComPort Error       |
| -12  | NACK Error          |
