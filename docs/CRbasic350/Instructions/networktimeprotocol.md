# NetworkTimeProtocol (Synchronize Clock with Internet Time)

The NetworkTimeProtocol function is used to synchronize the datalogger's clock with an Internet time server.

## Syntax

_variable_ = NetworkTimeProtocol(NTPServer,NTPOffset,NTPMaxMSec)

The simple program following example compares the datalogger's clock to an Internet time server every 24 hours. An offset of -25200 (7 hours) is used, which equates to mountain standard time.

The NetworkTimeProtocol function can also be run in a slow sequence to keep the datalogger's time synchronized with an Internet clock without adversely affecting performance in the main datalogger program.

```
Public TimeOffset As Long

BeginProg
Scan (1,Sec,3,0)
If IFTime (0,24,Hr) Then
TimeOffset =NetworkTimeProtocol("132.163.4.103", -25200,1000)
EndIf
NextScan
EndProg
```

## Remarks

Network Time Protocol, or NTP, is an internet protocol that can be used for clock synchronization between devices over a network. The protocol uses Coordinated Universal Time (UTC) as the basis for the synchronization. The datalogger can use network time protocol (NTP) to set its clock, and it can also act as an NTP server.

The NetworkTimeProtocol function returns the measured clock error in milliseconds, prior to the clock adjustment. If the command fails,NAN

**Note:** Not a number. A data word indicating a measurement or processing error. Voltage overrange, SDI-12 sensor error, and undefined mathematical results can produce NAN.

is returned. The timeout for this instruction is 70 seconds; the command will fail if no response is received within the timeout period.

If the IP address of the NTPServer parameter is a null string, "", it will act an an NTP server rather than a client.

## Parameters

# NTPServer (Internet Time Server)

The IP address or qualified domain name for the Internet time server. The entry must be enclosed in quotes. If a domain name is used, the IP address for a DNS must be set in the datalogger.

Enter a null string, "", if the NetworkTimeProtocol function is being used only to establish a time offset when the datalogger acts as an NTP server.

Type: Constant, enclosed in quotes

# NTPOffset (Universal Time Offset)

The NTPOffset parameter is used to specify an offset, in seconds, from Universal Time that should be used when the clock is set; for example, Mountain Standard Time in the U.S. is -7GMT, so the offset would be -25,200 (-7\* 3600). If the datalogger setting "UTC Offset" is set to a value other than -1 (disabled), this offset parameter will be ignored and the UTC Offset value in the datalogger will be used.

Type: Constant or Variable

# NTPMaxMSec (Max Time from Internet Time)

The maximum time, in milliseconds, that the datalogger's clock can differ from the Internet time server without triggering a clock set.

Type: Constant or Variable
