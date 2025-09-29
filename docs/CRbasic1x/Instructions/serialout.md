# SerialOut (Serial Data Outgoing Port)

The SerialOut function is used to transmit a string over one of the datalogger's communication ports. To transmit binary data, use [SerialOutBlock](serialoutblock.md).

## Syntax

```
SerialOut
```

(ComPort,OutString,WaitString,NumberTries,TimeOut)

In the following example program, a serial string is sent out COM port 1 (control port 1) and received back into COM port 3 (control port 4).The SerialOut function is used to send the data out C1.

```
'Declare Public Variables
Public RefTemp, batt_volt
Public Counter
Public OutString as string * 1000
Public InString as string * 1000

'Define Data Tables
DataTable (Test,1,-1)
DataInterval (0,15,Sec,10)
Minimum (1,batt_volt,FP2,0,False)
Sample (1,RefTemp,FP2)
Sample (1,OutString,String)
EndTable

'Main Program
BeginProg

'Set up communication ports to send and receive data
'jumper wire fromC1 (COM1 TX) to C4 (COM3 RX)
SerialOpen(ComC1,115200,0,0,10000)
SerialOpen(ComC3,115200,0,0,10000)

Scan (1,Sec,0,0)
PanelTemp (RefTemp,15000)
Battery (Batt_volt)

counter = counter + 1
OutString = "enter output string here " + counter

'Send String over control port C1(COM1 TX).
SerialOut(ComC1,OutString,"",0,100)
'Receive String over comntrol portC4 (COM3 RX).
SerialIn(InString,ComC3,100,0,1000)

'Call Output Tables
CallTable Test
NextScan
EndProg
```

## Remarks

This function returns the following:

- **0 ** if the ComPort is not opened through settings or [SerialOpen](serialopen.md).

- **0 ** if any portion of the WaitString is not received before the TimeOut occurs.

- The number of characters in WaitString if it is received in full before TimeOut occurs.

- The number of characters sent (OutString) if NumberTries and the TimeOut are**0 **.

- The number of characters successfully echoed before TimeOut occurs, if the WaitString is NULL and TimeOut and NumberTries are non-zero.

If a delay is needed before outputting the string, it should be entered in the TXDelay parameter of the [SerialOpen](serialopen.md) function. If the OutString and WaitString variables are not formatted as a string, they are converted to a string by the datalogger.

One of three conditions determines when the datalogger should proceed to the next instruction: when the WaitString is received, the NumberTries is exhausted, or the TimeOut is met.

When this function is used to send table data out the serial port (by loading the OutString array with a data table record using GetRecord), the [Tablename.Timestamp](tablenametimestamp.md) function can be used to include a timestamp in the data string.

** NOTE:**This function runs sequentially from the processing task sequencer, regardless of whether the datalogger is in pipeline or sequential mode. When SerialIn, SerialOut, SerialInBlock, and/or SerialOutBlock are used to access a COM port, the instructions should be placed in the same sequence (main program or slow sequence) to avoid unintentional concurrent reads or writes of that port.

This instruction can also be used along with other serial instructions to set up and control the [SDM-SIO1A or SDM-SIO4A](sdmsio1a_sdmsio4a.md)[or SDM-SIO2R](sdmsio2r.md).

## Parameters

# ComPort_Serial_Out (Communications Port Serial Out)

The COM port that will be used by the instruction. Right-click to display a list.

| Alphanumeric | Description                                |
| ------------ | ------------------------------------------ |
| ComRS232     | RS232 port of the datalogger               |
| ComME        | Datalogger's CS I/O port; modem enabled    |
| Com310       | Datalogger's CS I/O port; COM310 modem     |
| Com320       | Datalogger's CS I/O port; COM320 modem     |
| ComSDC7      | Datalogger's CS I/O port; SDC 7            |
| ComSDC8      | Datalogger's CS I/O port; SDC 8            |
| ComSDC10     | Datalogger's CS I/O port; SDC10            |
| ComSDC11     | Datalogger's CS I/O port; SDC 11           |
| ComC1        | Datalogger's control ports 1 (TX) & 2 (RX) |
| ComC3        | Datalogger's control ports 3 (TX) & 4 (RX) |
| ComC5        | Datalogger's control ports 5 (TX) & 6 (RX) |
| ComC7        | Datalogger's control ports 7 (TX) & 8 (RX) |

SerialOpen, SerialClose, SerialOut, and SerialOutBlock also support output via SDE pin enabled or any SDC address by using extended ComPorts. Valid extended ComPorts are &H1F through &Hff (31..255, SDC address including mode bits. Note, however, that 31 is SDE pin enabled and 32 through 47 are used to address the SDM-SIO1A). When an extended ComPort is used, the output instructions will turn on SDE or address the CS I/O port with the specified SDC address, delay the amount of time specified by TXDelay, output asynchronously at the specified baud rate in the SerialOpen() instruction, delay the amount of time specified by TXDelay, then reset the CS I/O port.

** NOTE:**The Argos and GOES satellite extended ComPort address is &H41 (65). To specify an SDC address, the most significant bit is the address; e.g., SDC 7 would be &H70 (112).

** NOTE:**When using the ComME ComPort with non-PakBus protocols (PPP, ModBus, DNP3, or generic serial applications), incoming characters can be corrupted by concurrent use of the CS I/O port for SDC communication (e.g., keyboard display).

Type: Constant. For all instructions except SerialOpen, this parameter can also be a variable.

A variable can be used in the ComPort parameter for use with functions that return a communication port. If communication occurs using TCP/IP, enter the variable for the socket returned by [TCPOpen](tcpopen.md).

# OutString (Outgoing String)

The variable to be sent out the serial port. If the data type being transferred is not a string, it will be formatted as one.

OutString can be null ("") if you don't want to send a string but want to wait for an incoming string before proceeding to the next instruction.

Type: Variable

# WaitString (Wait String)

An incoming string for which the datalogger should wait before proceeding to the next instruction after the OutString has been sent. The WaitString will be formatted as a string by the datalogger if it is not already in that format.

A null ("") WaitString can be entered to tell the datalogger to wait for the echo of each character in the OutString. The datalogger sends the OutString one character at a time and looks for the echo of each character. If the TimeOut is 0, the datalogger does not wait for the WaitString or echo of the OutString. The datalogger will simply send each character of the OutString the NumberTries and move on to the next instruction. If TimeOut > 0, the datalogger will wait for an echo of each character sent in the OutString, failing only when the TimeOut and NumberTries are met.

Type: Variable

# NumberTries (Number of Tries)

The total number of times the datalogger should attempt to send the OutString. If NumberTries is 0, the OutString is sent only once without checking for a WaitString. The length of the string sent is returned. If NumberTries is 1 the OutString is sent only once but the instruction checks for the WaitString. If the WaitString is received the length of the WaitString is returned.

If the WaitString or echo of the OutString is received before the NumberTries is met, the communication is successful and the datalogger executes the next instruction. If the WaitString is null, the datalogger will wait the NumberTries for an echo of each character output. If a negative NumberTries is entered and the WaitString is null, the datalogger will exit the instruction after the first failure of an echo (where failure = the amount of time specified in the TimeOut parameter has elapsed prior to receiving the echoed character).

Type: Constant, Variable, or Integer

# TimeOut (Wait Time)

For the SerialIn instruction, the TimeOut parameter is used to specify the amount of time, in 0.01 seconds, that the datalogger should wait before proceeding to the next instruction. If a 0 is entered for this parameter, the datalogger will wait for the termination character (TerminationChar parameter) or maximum number of characters (MaxNumChars parameter) to be received before proceeding. TimeOut is reset with each new incoming character.

For the SerialOut instruction, the TimeOut parameter is used to specify the amount of time, in 0.01 seconds, that the datalogger should wait for the WaitString or echo of each character in the OutString. If the TimeOut is 0, the datalogger does not wait (or even check) for the WaitString or Echo before proceeding to the next instruction. The TimeOut applies to each of the NumberTries; the overall timeout period for the instruction is cumulative. TimeOut is reset with each new incoming character.

As a result of internal buffering in the datalogger and/or external interfaces, data may not appear in the serial port buffer for a period of up to 50 ms (depending on the serial port being used). If the TimeOut is set at too short of an interval, the instruction may time out even though data has been received in the internal buffer (though not yet passed on to the serial port buffer). This should be taken into consideration when setting the TimeOut parameter.

Type: Constant
