# SerialInRecord (Incoming Serial Data)

The SerialInRecord instruction reads incoming serial data on a COM port and stores the data in a destination variable.

## Syntax

```
SerialInRecord
```

(COMPort,Dest,BeginWord,NBytes,EndWord,NBytesReturned,SerialInRecOption)

The following example programs show the use of the SerialInRecord instruction. One datalogger is used to send a serial string out COM1 and the string is parsed by the SerialInRecord instruction.

You can test this functionality by loading SerialOutRecord program into one datalogger and SerialInRecord program into another datalogger. Connect the two dataloggers' COM1 ports (C1 & C2), ensuring they have a common ground. In the software's numeric display for the SerialOutRecord datalogger, type in a string for the MyString variable. Any portion of the string preceded by a % sign and followed by a carriage return line feed is stored in the Destination variable (SerialInDest) in the datalogger running SerialInRecord.

As an example, if ABC is typed, nothing is stored in the SerialInRecord datalogger's Destination variable. However, if %ABC is typed in, ABC is stored.

This example shows the use of COM1 and COM2 as the name for the COM ports. For some datalogger models, the COM port name may need to be modified to reflect the specifications of the datalogger.

SerialOutRecord Program

```
Public StringtoSend As String * 100, MyString As String * 100
Const CRLF=CHR(13) + CHR(10)

BeginProg
SerialOpen (COM1,115200,0,0,10000)
Scan (1,Sec,3,0)

StringtoSend = MyString + CRLF
SerialOut (COM1,StringtoSend,"",0,100)

NextScan
EndProg
```

SerialInRecord Program

```
Public SerialIndest As String * 100, Receive, NBytesReturned

BeginProg
SerialOpen (COM1,115200,0,0,10000)

Scan (1,Sec,3,0)
'37 is the ASCII character for %
'&H0D0A is the hexadecimal representation for carriage return/line feed
SerialInRecord(COM1,SerialInDest,37,0,&H0D0A,NbytesReturned,01)
NextScan
EndProg
```

## Remarks

The SerialInRecord instruction can be used to read in and parse a string/record from a serial sensor. The BeginWord and EndWord parameters are used to define the beginning and/or ending of each string/record.

This instruction is typically handled by the digital task sequencer in the datalogger. However, if the ComPort parameter is a variable, the instruction is handled by the processing task.

The SerialOpen or TCPOpen instruction is used to set the size of the serial buffer in which the incoming data is held until it is stored into Dest. The buffer should be large enough to hold all of the data that can come in between calls to the SerialInRecord. In most cases data records are received by the datalogger asynchronous with the datalogger's scan rate (or rate of execution of SerialInRecord). The buffer size should be at least the size of two records plus one byte if the data records are coming in faster than the scan rate or at least one record plus one byte if the records are coming in slower than the scan rate. (The additional byte avoids the situation where if the number of incoming bytes stored in the buffer is equal to the buffer size, the datalogger may not detect that new data has been received in the buffer.) The size of a record includes the BeginWord, EndWord, and all bytes in between.

Multiple SerialInRecord instructions can be used in the program so that more than one sensor can be read via a single COM port, or multiple responses from one sensor can be read and stored separately. The SerialInRecordOption parameter is used to determine if the instructions will use one memory read pointer (global) for all incoming records or if memory pointers are tracked with each call of the instruction (local). For multiple sensors or responses through the same COM port, a local pointer should be used.

Examples of using multiple SerialInRecord instructions to process data received on a single COM port are:

- An array-based datalogger sending ASCII data serially to a PakBus datalogger, where the data contains multiple output arrays. The BeginWord would be the array ID to parse with the instruction, and the EndWord would be CR/LF.

- A GPS unit sending multiple string types, where each type needs to be parsed into its own array.

For incoming data, extended ASCII characters (128-255) are supported only by SerialOpen format options 1-7 and 17-23.

## Parameters

# ComPort_Serial_In (Communications Port Serial In)

The COM port that will be used by the instruction.Note that control ports C1 and C2 can be configured individually for transmit (Tx) or receive (Rx).Right-click to display a list.

| Alphanumeric | Description                                |
| ------------ | ------------------------------------------ |
| ComUSB       | USB port of the datalogger                 |
| ComRS232     | RS232 port of the datalogger               |
| Com1         | Datalogger's control ports 1 (TX) & 2 (RX) |
| Com2         | COM2 (TX & RX) port of the datalogger      |
| Com3         | COM3 (TX & RX) port of the datalogger      |
| ComC1_Rx     | Configures control port 1 for receive only |
| ComC2_Rx     | Configures control port 2 for receive only |
| ComRF        | Integrated RF communication                |

Type: Constant. For all instructions except SerialOpen, this parameter can also be a variable. However, if a variable is used, the port must be opened previously by [SerialOpen (Open Communication Port)](serialopen.md)

A variable can be used in the ComPort parameter for use with functions that return a communication port. If communication occurs using TCP/IP, enter the variable for the socket returned by [TCPOpen](tcpopen.md).

# Dest (Destination)

The Variable in which to store the results of the instruction. Right-click the parameter to display a list of defined variables.

If this instruction has a Repetitions parameter and it is greater than 1, the results are stored in an array with the variable name. The array must be dimensioned large enough to hold all of the values returned from all of the Reps.

Type: Variable or Array

# BeginWord (Beginning of Record)

A two byte word (1 to 65535) that indicates the beginning of a record. If the value entered is less than 256, the datalogger will look only for a single byte. If 0 is entered, a BeginWord is not used and the data stored in Dest will be NBytes prior to the EndWord. If &H80000000 is entered, a Null character will be used for BeginWord.

Hexadecimal values can be entered for BeginWord and EndWord by preceding the characters with &H.

Type: Constant integer

# NBytes (Number of Bytes)

The number of bytes that should be stored in Dest after the BeginWord has been received. If NBytes is equal to or less than 0, then bytes are read between the BeginWord and the EndWord, exclusive of the BeginWord and EndWord. If the BeginWord is not used (BeginWord = 0) then this parameter is the number of bytes to store prior to the EndWord.

Type: Constant

# EndWord (End Record)

A two byte word (1 to 65535) that indicates the end of a record. If 0 is entered, an EndWord is not used and the data stored in Dest will be NBytes after to the BeginWord. If &H80000000 is entered, a Null character will be used for EndWord. If the value entered is less than 256, the datalogger will look only for a single byte.

Hexadecimal values can be entered for BeginWord and EndWord by preceding the characters with &H.

Type: Constant integer

# NBytesReturned (Number of Bytes Returned)

A variable in which the number of bytes read by the instruction is stored, not including the BeginWord or EndWord. If the number of bytes received is too large to fit into Dest, a negative value will be returned in this parameter.

Type: Variable

# SerialInRecOption (Serial In Record)

Determines what record from the serial buffer is stored into Dest and whether the datalogger loads aNAN

**Note:** Not a number. A data word indicating a measurement or processing error. Voltage overrange, SDI-12 sensor error, and undefined mathematical results can produce NAN.

value or if the last value loaded will remain. Option codes include:

| Code | Description                                                         |
| ---- | ------------------------------------------------------------------- |
| 00   | Most recent record in serial buffer, do not store NAN if no records |
| 01   | Most recent record in serial buffer, store NAN if no record         |
| 10   | Oldest record in serial buffer, do not store NAN if no record       |
| 11   | Oldest record in serial buffer, store NAN if no record              |

Add 100 to the Code to use a local read pointer (i.e., 100, 101, 110, 111).

If option 01 or 11 is chosen and no new record is received, the NAN value stored in Dest will depend upon its data type. If Dest is formatted as a float, NAN will be stored. If it is formatted as a LONG, &H80000000 will be stored. If Dest is formatted as a string, a NAN will be stored.

[SeeASCII Codes and Charactersfor more information.](../Info/CodesChar.md)

**NOTE:** Hexadecimal values can be entered for BeginWord and EndWord by preceding the characters with &H. This instruction can be run in a conditional statement only in SequentialMode or a SlowSequence scan.
