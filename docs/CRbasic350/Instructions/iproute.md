# IPRoute (IP Route)

The IPRoute instruction is used to set the interface to be used when the datalogger sends an outgoing packet and multiple interfaces are active.

## Syntax

IPRoute(IPAddr,IPInterface,ExclusiveOption[optional] )

The following program shows the use of the IPRoute instruction to change the IP interface from the default to PPP.

```
Public pppSocket As Long,pppIP As String, RemoteBattery,ResultCode As Long
Const IPAddress = "192.168.4.23"

BeginProg
Scan (5,Sec,0,0)
Do
pppIP = PPPOpen
Loop Until (pppIP <> "0.0.0.0")
IPRoute(IPAddress,1)'add a route to IPAddress via PPP interface
pppSocket = TCPOpen(IPAddress,6785,0)
If pppSocket Then GetVariables(ResultCode,pppSocket,0,1,0,0,"Status","Battery",RemoteBattery,1)
NextScan
EndProg
```

## Remarks

When multipleIP

**Note:** Internet Protocol. A TCP/IP internet protocol.

interfaces (e.g., CS I/O port, Wi-Fi, PPP) are active, the IPRoute instruction is used to direct outgoing IP traffic such as email, FTP, or HTTP through a specified port.

IPRoute may also be used to specify the interface to use to reach the DNS servers needed to resolve domain names.

For example:

> ```
> IPRoute("x.x.x.x",1,1) 'DNS Address 1
> ```
>
> ```
> IPRoute("x.x.x.x",1,1) 'DNS Address 2
> ```
>
> ```
> IPRoute("x.x.x.x",1,1) 'DNS Address 3
> ```
>
> ```
> IPRoute("cbaex.konectgds.com",1,1) 'KonectGDS address the datalogger wants to reach
> ```
>
> ```
> IPRoute("kdapiemail.konectgds.com",1,1) 'Email Server the datalogger wants to reach
> ```
>
> Where the x s represent the IP octets of the DNS server IP address.

If the IPRoute instruction is placed within a Scan/NextScan, the instruction will not be executed at compile time, but rather when the instruction is encountered in the scan. If IPRoute appears between BeginProg and Scan, the instruction will be executed only at compile time.

## Parameters

# IPAddr (IP Address)

The IP address for the socket you are trying to open. This is a string variable, which can be entered as a numeric address (for example, "xxx.xxx.xxx.xxx", with each xxx being a value of 0 to 255), or an IPv6 address, or a fully-qualified domain name (for example, "computer-name.domain.com"). Numeric IPv4 addresses should be entered in decimal notation, with no leading zeros (i.e., 192.168.**1 **.123, not 192.168.** 001 **.123). If you use a domain name, the address of aDNS

** Note:**Domain name server. A TCP/IP application protocol.

server must be specified in datalogger settings. For all instructions except UDPOpen and IPRoute, the IPAddr can also be set to a null string (""), in which case the datalogger will listen for an incoming TCP/IP connection on the specified port. The entry for the IPAddr must be enclosed in quotes.

Type: Variable defined as a string, enclosed in quotes

Specifying empty quotes ("") for the IPAddr sets the specified IPInterface as the default network (gateway).

# IPInterface (IP Interface)

Specifies the active interface to use for the outgoingIP

** Note:**Internet Protocol. A TCP/IP internet protocol.

communication. Right click for a drop-down list of options.

- 1 = PPP

- 2 = USB

- 4 = Wi-Fi

Type: Constant

## Optional Parameter

# ExclusiveOption

Optional parameteradded with OS8. Determines if the datalogger will attempt a connection through another IPinterface, if the attempt to route through the specified IPinterface fails. The ExclusiveOption is useful if more than one active interface is available (e.g., PPP and Ethernet) but the specified interface should be used exclusively for this outgoing communication.

- 0 or absent = Allow routing through another active interface

- 1 = Do not allow routing through another active interface

Type: Constant
