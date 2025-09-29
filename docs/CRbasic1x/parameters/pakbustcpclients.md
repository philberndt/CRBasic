# PakBusTCPClients (PakBus TCP Clients)

SetSetting("PakBusTCPClients", Addresses)

The datalogger can be set to maintain a connection to up to four PakBus TCP/IP devices, or "PakBusTCPClient" connections. This syntax configures the setting programmatically. The Addresses entry for each connection includes the destination IP address and port number. Valid delimiters for separating elements are space, tab, comma, semi-colon, open parenthesis, and close parenthesis. The setting name "PakBusTCPClients" is not case sensitive.

Calling SetSetting() in the program will cause the datalogger to recompile the program.

# Example:

SetSetting("PakBusTCPClients","(www.myloggernet.com,6785)(1.1.1.1,6785)(another.station.com,6785)(2.2.2.2,6785)")
