# PingIP (Ping IP Address)

The PingIP function is used to ping an IP address.

## Syntax

_Variable _ = PingIP(PingIPAddr,PingTimeOut,PingOption[optional] )

**NOTE:** In order for PingIP to work, the Ping Enabled setting must be set to 1 in the data logger settings.

In the following program, if Flag(1) is toggled high the datalogger will attempt to ping a device at the address of 192.168.7.76. The results are returned into the variable PingMe. For testing purposes, IPTrace is used to store trace messages to a string variable.

In a real-world program, PingIP might be used to see if a device is available on the network prior to contacting it for some other reason (such as sending the device a file via FTP).

```
Public PingMe, Flag(2) As Boolean, TraceMessage As String * 100

BeginProg

Scan (1,Sec,3,0)
If Flag(1) Then
PingMe=PingIP("192.168.7.76",100)
Flag(1) = False
IPTrace (TraceMessage)
EndIf
NextScan

EndProg
```

## Remarks

The PingIP function returns the response time (in milliseconds) if a response is received from the ping within the timeout period; otherwise, it returns 0.

**NOTE:** There is also a PING command available in the data loggers Terminal Mode, which can be used for testing and troubleshooting. For example: CRX> PING <HOST>. The host can be an IP address or a domain name, such asCR350>PING CAMPBELLSCI.COM.

## Parameters

# PingIPAddr (IP Address to Ping)

The IP address of the device to be pinged. This is a string variable, which can be entered as a numeric address (for example, "xxx.xxx.xxx.xxx", with each xxx being a value of 0 to 255) or a fully-qualified domain name (for example, "computer-name.domain.com"). The entry for the IPAddr must be enclosed in quotes.

Type: String Variable

# PingIPTimeOut (Timeout)

The time, in milliseconds, that the datalogger should wait for a response back from the pinged device. If a response is not received within the timeout period, 0 will be returned.

Type: Constant or variable

## Optional Parameter

# PingOption

Optional parameter that specifies the IP version of the address to return if the address is a domain name that needs to be resolved to a numeric IP address. The optional parameter may be configured as follows:

| Option Code | Description                       |
| ----------- | --------------------------------- |
| 0 or absent | Ping IPv4 first, if fail try IPv6 |
| 1           | Ping only IPv4                    |
| 2           | Ping only IPv6 (local and global) |
| 3           | Ping IPv6 first, if fail try IPv4 |
