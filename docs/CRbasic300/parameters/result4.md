# Result

The Result parameter is a string variable that holds either the data to be output in its specified format or a message indicating there are no data to output to the transmitter. If the Result parameter is used to view data that is output to the transmitter, then the string variable must be sized large enough to accommodate the data to be viewed. Following is a list of example messages that may be returned when there are no data output to the transmitter:

Examples of messages when there is no data output to the transmitter:

- No Output

- Write error

- Timed out wating for STX character from transmitter after SDC addressing

- Wrong character received after SDC addressing

- Timed out waiting for ACK or OK

- CS I/O port not available; GOES not attached

- Buffer Control Error

- COM9602 not communicating

- Illegal data format

- Transmitter not set up

Type: Variable of type string
