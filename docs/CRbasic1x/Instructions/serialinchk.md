# SerialInChk (Number of Serial In Characters)

The SerialInChk function is used to return the number of characters available in the datalogger's serial buffer, which can be read by SerialIn or SerialInBlock.

## Syntax

```
SerialInChk
```

(ComPort)

In the following example, serial data that is received over the datalogger's RS232 port is stored into a string. After 20 characters are received, the remaining serial buffer is cleared using the SerialFlush function. SerialInChk is used to monitor the number of characters received.

```
Public InString As String * 25

DataTable (SerialT,True,-1)
Sample (1,InString,String)
EndTable

BeginProg
Scan (5,Sec,3,0)
SerialOpen (ComRS232,-115200,0,0,10000)
IfSerialInChk(ComRS232) > 20 Then SerialFlush (ComRS232)
SerialIn (InString,ComRS232,100,0,20)
CallTable (SerialT)
NextScan
EndProg
```

## Remarks

A variable can hold the result of the SerialInChk function (variable = SerialInChk(Comport)) or the function can be used in an expression (If SerialInChk(Comport)>20 Then ). If the serial port has not been opened (using SerialOpen), a -1 is returned. If the serial port buffer overflows before it has been read, it is reset to 0 and the remainder of the bytes available is returned by the function. For instance, if you have a buffer of 100 bytes, and 120 bytes come in before the buffer is read, SerialInChk returns a value of 20 (the last 20 bytes received). In this instance, there is no way to detect that the buffer has overflowed and been reset.

Beginning with OS7.0, SerialInChk() will flush the hardware buffer to the serial buffer immediately when the SerialInChk() fiunction is executed. Prior to the OS change, data may not appear in the serial port buffer until there is at least 50 ms of idle communication time. If the SerialIn instruction's TimeOut is set at too short of an interval, the instruction may time out even though data has been received in thehardware buffer (though not yet passed on to the serial port buffer). This should be taken into consideration when setting that instruction's TimeOut parameter.

This instruction can also be used along with other serial instructions to set up and control the [SDM-SIO1A or SDM-SIO4A](sdmsio1a_sdmsio4a.md)[or SDM-SIO2R](sdmsio2r.md).

## Parameter

# ComPort_Serial_In (Communications Port Serial In)

The COM port that will be used by the instruction. Right-click to display a list.

| Alphanumeric | Description                                |
| ------------ | ------------------------------------------ |
| ComRS232     | RS232 port of the datalogger               |
| ComMe        | Datalogger's CS I/O port;modem enabled     |
| Com310       | Datalogger's CS I/O port; Com310 modem     |
| Com320       | Datalogger's CS I/O port; Com320 modem     |
| ComSDC7      | Datalogger's CS I/O port; SDC7             |
| ComSDC8      | Datalogger's CS I/O port; SDC8             |
| ComSDC10     | Datalogger's CS I/O port; SDC10            |
| ComSDC11     | Datalogger's CS I/O port; SDC11            |
| ComC1        | Datalogger's control ports 1 (TX) & 2 (RX) |
| ComC3        | Datalogger's control ports 3 (TX) & 4 (RX) |
| ComC5        | Datalogger's control ports 5 (TX) & 6 (RX) |
| ComC7        | Datalogger's control ports 7 (TX) & 8 (RX) |

Type: Constant. For all instructions except SerialOpen, this parameter can also be a variable. However, if a variable is used, the port must be opened previously by [SerialOpen (Open Communication Port)](serialopen.md)

A variable can be used in the ComPort parameter for use with functions that return a communication port. If communication occurs using TCP/IP, enter the variable for the socket returned by [TCPOpen](tcpopen.md).
