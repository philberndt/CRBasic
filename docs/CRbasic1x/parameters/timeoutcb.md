# Timeout

Used to specify the maximum time, in seconds, for dialing and connecting to the LoggerNet server (or remote datalogger). Valid entries for Timeout are 10 through 120 seconds. If the Timeout is exceeded and no connection has been made the datalogger will enter its Retry mode. When the Timeout period expires, the modem is turned off until the next retry. After 10 unsuccessful retries, the instruction will abort the attempt to connect.

Type: Constant
