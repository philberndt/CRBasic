# IPTimeOut (IP Connection Timeout)

When acting as a TCP client, the IPTimeOut parameter specifies the amount of time the datalogger will spend attempting to initiate the socket connection before failing. The value is entered in centiseconds (i.e., 0.01 seconds). If omitted, the timeout default is 75 seconds (7500). When acting as a TCP server, this parameter specifies the maximum amount of time allowed to elapse without active communications once a connection is accepted. If the timeout expires, the datalogger will close the connection. If omitted, a timeout will not be used and the datalogger will not close the connection due to lack of communications.

Type: Constant Integer
