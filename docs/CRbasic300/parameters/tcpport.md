# TCPPort (TCP Port)

An integer value ranging from 1 to 65535 and may be specified as a constant or variable. When acting as a TCP client, this parameter specifies the destination port the datalogger will connect to on the remote device. When acting as a server, this parameter specifies the port the datalogger will listen on for incoming TCP connections. When acting as a server, care should be taken to not use a port number already in use by other datalogger services such as FTP (20/21), Telnet (23), HTTP (80), NTP (123), SNMP (161), HTTPS (443), Modbus (502), PakBus (6785), or DNP3 (20000).Caution: If you have set up the datalogger to listen for an incoming TCP/IP connection, the TCPPort parameter should be different than the TCPort setting in the datalogger (6785 by default), since the datalogger is already listening on that port.

Type: Variable or Constant
