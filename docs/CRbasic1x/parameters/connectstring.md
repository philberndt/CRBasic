# ConnectString

Used to specify the response code expected back from the modem when a connection is made. The ConnectString will succeed even when a partial response string is entered (for instance, a ConnectString of "Connect" will be successful if the response code is "Connect", "Connect 9600", or "Connect 1200").

When using the COM220 modem, enter a null string (""). The COM220 can return one of eight responses, depending upon the baud rate at which the connection is made (1 Connect, 5 Connect1200, 10 Connect2400, 13 Connect9600, 18 Connect4800, 20 Connect7200, 21 Connect12000, 25 Connect14400). When a null string is entered, the datalogger will compare the modem's response with these eight known responses in an attempt to establish communication.

Type: String
