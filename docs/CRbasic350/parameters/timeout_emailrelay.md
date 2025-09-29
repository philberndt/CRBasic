# TimeOut (Response Wait Time)

An optional parameter that specifies how long the datalogger will wait (specified in 0.01 seconds) before considering the attempt to connect to the server failed and returning the appropriate result code. The default TimeOut in the absence of this parameter is 7500 (i.e., 75 seconds). To specify a custom timeout without using the other optional parameters (that is, if not streaming data), use commas as placeholders for the unused parameters. For example,**EmailRelay**( ToAddr, "Subject", "Message", "Attach", Result,,,,,,500). In this example, 6 commas precede 500, which sets the timeout to 5 seconds.

Type: Constant
