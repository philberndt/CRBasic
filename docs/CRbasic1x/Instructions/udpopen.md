# UDPOpen (Open Port for UDP)

The UDPOpen function opens a port for transferring UDP packets.

## Syntax

UDPOpen(IPAddr,UDPPort,IPBuffer,IPVersion[optional] )

## Remarks

This function returns a non-zero long integer if the socket is created. If the socket fails to be created a 0 is returned. Once a port is opened, serial input and output instructions can be used to transfer packets.

UDPOpen can be placed in a slow sequence scan. In a slow sequence, it will run in the background and will not hold up the main program waiting for the socket to be opened successfully.

## Parameters

# IPAddr (IP Address)

The IP address for the socket you are trying to open. This is a string variable, which can be entered as a numeric address (for example, "xxx.xxx.xxx.xxx", with each xxx being a value of 0 to 255), or an IPv6 address, or a fully-qualified domain name (for example, "computer-name.domain.com"). Numeric IPv4 addresses should be entered in decimal notation, with no leading zeros (i.e., 192.168.**1 **.123, not 192.168.** 001 **.123). If you use a domain name, the address of aDNS

** Note:**Domain name server. A TCP/IP application protocol.

server must be specified in datalogger settings. For all instructions except UDPOpen and IPRoute, the IPAddr can also be set to a null string (""), in which case the datalogger will listen for an incoming TCP/IP connection on the specified port. The entry for the IPAddr must be enclosed in quotes.

Type: Variable defined as a string, enclosed in quotes

# UDPPort (UDP Port Number)

The port number over which the datalogger will communicate. The valid range is 0 through 65535. In most instances, the value should be 49151 or greater. Values less than this are used by other common applications.

Type: Variable or Constant

# IPBuffer (Input Buffer)

If the socket is being opened for communication with a serial (non-PakBus

** Note:**PakBus is a proprietary communication protocol developed by Campbell Scientific to facilitate communications between Campbell Scientific devices. Similar in concept to IP (Internet Protocol), PakBus is a packet-switched network protocol with routing capabilities. A registered trademark of Campbell Scientific, Inc.

) device, enter a value for the size of the input serial buffer that should be created in datalogger memory. Set this parameter to 0 for communication with a PakBus device. When this parameter is set to 0, the port will be opened as a PakBus port and PakBus beaconing will take place every 3 minutes. For serial communications with non-PakBus devices, specify a buffer size large enough to hold the number of bytes that will arrive between serial input reads. Note that oversizing the buffer needlessly takes away from datalogger memory. For non-PakBus communication, do not set this parameter to 0 or PakBus beacons initiated by the datalogger may interfere with communications. For non-PakBus communication that does require a user-specified buffer (such as when used with [ModbusClient()](modbusclient.md), set this parameter to 1 so that beaconing does not occur.

Type: Constant

** Caution:**For the UDPOpen instruction, do not set this parameter to 0 or the datalogger will attempt PakBus communication over the port and beaconing will interfere with communication. For non-PakBus communication that does not require a buffer set this parameter to 1 so that beaconing does not occur

## Optional Parameter

# IPVersion (IPV Address Type)

Optional parameter used to specify an IPV4 or IPV6 address type when the IPAddr parameter is null (and thus, the datalogger is listening on the port rather than initiating the connection). 0 = IPV4, 1 = IPV6.

Type: Constant
