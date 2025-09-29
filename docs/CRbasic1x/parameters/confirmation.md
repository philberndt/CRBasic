# Confirmation (DNP Confirmation)

A parameter that is used to determine whether DNP3 data link layer confirmation is enabled or disabled, and to set the timeout interval for data link layer and application layer confirmation. In general, the use of data link layer confirmation is not recommended. Data link layer confirmation is always disabled for a TCP/IP link. The datalogger will always use application layer confirmation when transmitting event data or multi-fragment responses. The parameter is entered in the form of XSSS. X = 0 enables data link layer confirmation. X = 1 disables data link layer confirmation. SSS is the number of seconds the datalogger should wait for a response to data link layer confirmation and/or application layer confirmation before timing out. A timeout > 0 should always be entered.

Examples:

- **1010 **: Disable link layer confirmation and set the application layer confirmation timeout to 10 seconds.

- ** 0006**: Enable data link layer confirmation and set data link layer confirmation and application layer confirmation timeout to 6 seconds.

When using ComRS232, the datalogger usually goes into sleep mode after 40 seconds of inactivity on the communications port. After going to sleep with some interface methods it sometimes takes a packet of incoming data to wake it up and then a retry packet to get the message through. If packets continue arriving before the 40 second timeout, the datalogger should respond very quickly to the new packets.

Type: Constant or Variable
