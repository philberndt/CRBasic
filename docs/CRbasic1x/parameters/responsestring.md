# ResponseString

Used to specify the response code expected back from the modem when a connection is made. An entry for this parameter is required.

When using the COM220 modem, enter a null string (""). The COM220 can return one of eight responses, depending upon the baud rate at which the connection is made. When a null string is entered, the datalogger will compare the modem's response with these eight known responses in an attempt to establish communication.

Type: String
