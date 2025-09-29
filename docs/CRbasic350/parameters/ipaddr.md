# IPAddr (IP Address)

The IP address for the socket you are trying to open. This is a string variable, which can be entered as a numeric address (for example, "xxx.xxx.xxx.xxx", with each xxx being a value of 0 to 255), or an IPv6 address, or a fully-qualified domain name (for example, "computer-name.domain.com"). Numeric IPv4 addresses should be entered in decimal notation, with no leading zeros (i.e., 192.168.**1 **.123, not 192.168.** 001 **.123). If you use a domain name, the address of aDNS

** Note:**Domain name server. A TCP/IP application protocol.

server must be specified in datalogger settings. For all instructions except UDPOpen and IPRoute, the IPAddr can also be set to a null string (""), in which case the datalogger will listen for an incoming TCP/IP connection on the specified port. The entry for the IPAddr must be enclosed in quotes.

Type: Variable defined as a string, enclosed in quotes
