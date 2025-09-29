# UDPDataGram (Send Information via UDP)

The UDPDataGram function is used to send packets of information via the UDP communications protocol.

## Syntax

UDPDataGram(IPAddr,UDPPort,UDPSendVar,UDPSendLen,UDPRecVar,UDPTimeOut,UDPConnectHandle[optional]IPVersion[optional] )

The following example program shows transmitting a UDP data gram upon a rain event.

This program was written for aCR350. The PulseCount instruction may need to be changed for other dataloggers, depending upon the datalogger's capabilities.

```
'Declare Constants
Const StationID=66048

Const UDPPrimary="123.123.123.123"

'Declare Public Variables
Public DataString as String *100
Public DataCRC
Public DataStringCRC as String *100
Public Rain
Public Batt_Volt
Public IPAddress as String *20
Public Rain_No_UDP as Boolean

'Define Data Tables
DataTable(TotalRain,True,-1)
Sample(1,Rain,IEEE4)
EndTable

'Define Subroutines
Sub Data_UDP
'Form the first part of the string
DataString = "Rainfall Tip occurred."'This is the event message string
'Now calculate a UKMO 16-bit CRC on the contents of the message
DataCRC = CheckSum(DataString,19,0)
'Now insert the CRC into the requisite part of the string
DataStringCRC = DataString+"CRC="+DataCRC+Chr(13)+Chr(10)
' UDP to primary IP address
' Format is UDPDataGram("IPAddress",UDP Port, SendBuffer, SendLength, RecvBuffer,Timeout) - as it turns out, the Port number can't be a string enclosed in quotes. Itmust be a number.
UDPDataGram(UDPPrimary,50500,DataStringCRC,100,0,0)
EndSub

'Main Program
BeginProg
Scan (1,Sec,0,0)
PulseCount (Rain,1,P_LL,1,0,1.0,0)
Battery(Batt_Volt)
IPAddress = PPPOpen()

'Has there been rain?
If Rain <> 0 then
CallTable TotalRain
Rain_No_UDP = True
EndIf

' If there has been rain check to see if PPP is open. If it is, then call the UDP subroutine.
If Rain_No_UDP = True and IPAddress <> "" and IPAddress <> "0.0.0.0" then
Call Data_UDP
EndIf

NextScan
EndProg
```

## Remarks

If [UDPOpen](udpopen.md) is used, this instruction is not necessary. However, this function is a convenient way to send UDP datagrams. The function allocates a UDP socket (if not already allocated) and sends a packet of the specified length.

UDPDataGram returns the number of bytes received if successful. It returns 0 if unsuccessful (for instance, if there is not enough memory to allocate a socket or if there is no gateway available on the network and the destination is not within the datalogger's IP network) or if it receives 0 bytes. It returns a -1 if successful but there is nothing to receive.

Multiple messages from different remotes are queued in the background. With every call to this function, it will return the first message in the queue into the UDPRecVar and update the connection handle so that any output until the next call to this instruction is sent to the source of the received message. If this instruction is not called regularly to service the incoming messages, the queue will grow until the datalogger runs out of memory.

Note that serial output using the handle returned by`UDPDatagram()`will only be successful if the call to UDPDatagram returned non zero for the number of characters received.

## Parameters

# IPAddr (IP Address)

The IP address for the socket you are trying to open. This is a string variable, which can be entered as a numeric address (for example, "xxx.xxx.xxx.xxx", with each xxx being a value of 0 to 255), or an IPv6 address, or a fully-qualified domain name (for example, "computer-name.domain.com"). Numeric IPv4 addresses should be entered in decimal notation, with no leading zeros (i.e., 192.168.**1 **.123, not 192.168.** 001 **.123). If you use a domain name, the address of aDNS

** Note:**Domain name server. A TCP/IP application protocol.

server must be specified in datalogger settings. For all instructions except UDPOpen and IPRoute, the IPAddr can also be set to a null string (""), in which case the datalogger will listen for an incoming TCP/IP connection on the specified port. The entry for the IPAddr must be enclosed in quotes.

Type: Variable defined as a string, enclosed in quotes

# UDPPort (UDP Port Number)

The port number over which the datalogger will communicate. The valid range is 0 through 65535. In most instances, the value should be 49151 or greater. Values less than this are used by other common applications.

Type: Variable or Constant

# UDPSendVar (UDP Send Variable)

Holds the value(s) that will be sent via the UDP communications protocol.

Type: Variable

# UDPSendLen (UDP Send Length)

The length, in bytes, of the packet to send. The maximum number of bytes is one IP packet (for example, ~1500 bytes).If UDPSendLen is 0, no packet will be sent.

Type: Constant

# UDPRecVar (UDP Receive Variable)

UDPRecVar is the variable that will store a UDP datagram that is received by the datalogger.

# UDPTimeOut (UDP Timeout Period)

The UDPTimeOut is the timeout period, in ms, for a UDP communications attempt. The timeout is in effect only when there are no messages in the received message queue.

Type: Constant

## Optional Parameters

# ConnectionHandle (Communication Port for Incoming UDP Data)

An optional parameter that returns the communications port (socket) of incoming UDP datagrams so that it can be used for subsequent serial out instructions.

Type: String variable

# IPVersion (IPV Address Type)

Optional parameter used to specify an IPV4 or IPV6 address type when the IPAddr parameter is null (and thus, the datalogger is listening on the port rather than initiating the connection). 0 = IPV4, 1 = IPV6.

Type: Constant
