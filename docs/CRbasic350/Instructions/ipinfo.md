# IPInfo (IP Information)

The IPInfo instruction is used to return the IP address of the specified datalogger interface into a string. The IPInfo instruction can also be used to return the remote IP address associated with an IP socket resource handle.

## Syntax

Variable =IPInfo(Interface,Option)

The following example shows using the IPInfo instruction to return the IP address from the datalogger.

```
Public IPInfoVar As String * 50

BeginProg
Scan (1,Sec,3,0)
'return the IP address of the Ethernet interface, in Ipv4 format
IPInfoVar =IPInfo(0,0)

NextScan
EndProg
```

## Remarks

The IP address can be requested in Ipv4 or Ipv6 format. If the network interface is not present, this function returns a blank string. If the interface is present but an IP address has not been assigned, it will return 0.0.0.0 . For an IPv6 address, this function returns the global address if present; otherwise, it returns the local address.

The IPInfo instruction can also be used to return the remote IP address associated with an IP socket resource handle. The function will return the IPv4 or IPv6 address of the remote host used for the connection. The Option parameter is ignored in this case.

## Parameters

# Interface (Device)

Selects the device to be read by the function. Right-click the parameter for a drop-down list of options:

| Code | Description |
| ---- | ----------- |
| 1    | PPP         |
| 2    | USB         |
| 4    | Wi-Fi       |

Type: Constant or Variable

# Option (IP Address Version)

Indicates what version of IP address to return from the datalogger. Right-click the parameter for a drop-down list of options:

| Code | Description |
| ---- | ----------- |
| 0    | IPv4        |
| 1    | IPv6        |

This parameter is ignored if the instruction is used to return the IP address associated with an IP socket resource handle.

Type: Constant or Variable
