# TCPClose (Close TCP/IP Communication Port)

The TCPClose function is used to close a TCP/IP socket that has been set up for communication.

## Syntax

TCPClose(TCPSocket)

The following program demonstrates the use of TCPClose.

```
Public Socket as Long
Const IP_Addr = "1.1.1.1"'Set the IP address to make a connection to

BeginProg
Socket = TCPOpen(IP_Addr,5000,100)'Create socket. Socket number typically = 101
Scan (2,Sec,3,10)
SerialOut (Socket,"wha-da-ya know?",0,1,0)'send out a string
NextScan

TCPClose(Socket)'Close the socket; this can take up to 200 msec

EndProg
```

## Remarks

TCPSocket is the variable returned by the [TCPOpen](tcpopen.md) function.

If TCPSocket is negated, TCPClose does not close the socket but instead allows PakBus communication to occur on the socket. This feature allows a socket to be opened first for Serial I/O, perhaps to "log in", and then converts the socket so that it can communicate via PakBus. (Note that if TCPSocket is opened by TCPOpen where IPBuffer is non-zero, the socket is opened for Serial I/O only and not for PakBus.)

TCPClose returns 0 if there is no IP interface (Ethernet or PPP).

If a positive TCPSocket is used, this function waits until the socket is closed and should always return 0. (It can take up to 200 msec to close the socket.)

If a negative TCPSocket is used, this function returns:

| Code | Description                                                                                                   |
| ---- | ------------------------------------------------------------------------------------------------------------- |
| 0    | The socket is not open or PakBus is already active on the socket.                                             |
| 1    | The socket is successfully converted to PakBus.                                                               |
| 3    | The socket is waiting for a client to authenticate the socket (when the PakBus/TCP setting is active).        |
| 4    | A challenge has been issued to the client to authenticate the socket (when the PakBus/TCP setting is active). |
