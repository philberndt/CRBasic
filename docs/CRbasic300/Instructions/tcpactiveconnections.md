# TCPActiveConnections

The TCPActiveConnections instruction is used to monitor information about TCP connections on a specified port. A common use-case for this instruction is to monitor polling of the datalogger by another device. For example, from a ModBus client or DNP master or from a web browser.

## Syntax

TCPActiveConnections(Dest,SecsSinceLastRx,Port)

The following program demonstrates using the TCPActiveConnections instruction to monitor polling of a datalgger by a DNPMaster on port 20000. To view information for all active TCP connections, change the port number to 0. The seconds since the last connection to the DNP master is stored in the LastRx_Secs variable. If more than one Master is polling the datalogger on port 20000, LastRx_Secs should be an array so that seconds since last Rx may be monitored for each master.

```
Public Binput(5) As Boolean
Public Dest as String *300, LastRx_Secs(10) as Long
Public Port as Long

'Main Program

BeginProg
Port = 20000

DNP(20000,115200,1000)'20000 is the default port for DNP over TCP
'DNPVariable(source,swath,DNPobject,DNPvariation,DNPclass,DNPflag,DNPEvent,DNPNumEvents)
DNPVariable (Binput(),5,1,1,0,&B00000000,0,0) 'Static data
DNPVariable (Binput(),5,2,1,1,&B00000000,0,9) 'Event data

Scan(1,Sec,1,0)
TCPActiveconnections(Dest,LastRx_Secs,20000)'specify port 20000 to display only active connection from DNP Test Harness on this port

DNPUpdate(1,10)
NextScan
EndProg
```

## Remarks

The information displayed for each connection is the IP address, TCP port, and the date and time of the last received communication from that IP address on the specified port. The seconds since the last communication received is 0 if the connection is active and increments if the connection becomes inactive. SecsSinceLastRx will be -1 if there is no connection.

## Parameters

# Dest

Variable of Type String in which to store the connection information. The information returned is the IP Address, port number, and time of last connection. If multiple connections are active, the most recent connection is listed first. The list auto re-organizes with the freshest connection at the beginning. The format of the list is: IP Address:xxx.xxx.xxx.xxx; Port: xxxx; Time: MM/dd/yyyy hour:min:secs:microsec. If multiple connections are listed, each is shown on a separate line. If the string variable is not sized large enough to hold all the connection information, then only the information that fits into the variable is shown.

# SecsSinceLastRx

A variable or variable array of Type Long which stores how many seconds it has been since something was last received on the specified connection. If the connection is active, this number is 0. If the connection becomes inactive, this number increments. If no connection is made, -1 is returned. If more than one connection is expected on the specified port, SecsSinceLastRX should be declared as an array and each element will contain seconds since last Rx for the corresponding connection in the Dest list. The list auto re-organizes with the freshest connections at the beginning.

# Port

Constant or variable of Type Long that specifies the TCP port to monitor. If 0 is entered for the port parameter, then all active TCP connections will be shown.
