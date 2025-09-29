# SerialIn (Serial Data Incoming Port)

The SerialIn instruction is used to set up a communications port for receiving incoming serial data.

## Syntax

```
SerialIn
```

(Dest,ComPort,TimeOut,TerminationChar,MaxNumChars)

In the following example program, a serial string is sent out control port 1 and received back into control port 2.

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
'jumper wire fromC1 to C2
SerialOpen(COMC1_TX,115200,0,0,10000)
SerialOpen(ComC2_Rx,115200,0,0,10000)

Scan (1,Sec,0,0)
PanelTemp (RefTemp,4000)
Battery (Batt_volt)

counter = counter + 1
OutString = "enter output string here " + counter

'Send String over control port C1
SerialOut(COMC1_TX,OutString,"",0,100)
'Receive String over comntrol portC2
SerialIn(InString,ComC2_Rx,100,0,1000)

'Call Output Tables
CallTable Test
NextScan
EndProg
```

## Remarks

Incoming data is stored in the destination variable until the TerminationChar is received, MaxNumChars value is met, or the TimeOut parameter is exceeded. After the end condition is met, SerialIn will terminate the destination variable with a null. Strings used as termination characters are included in the destination variable; however, numeric characters are not. A null character will terminate the string, but any characters after the null will nevertheless continue to be received into the variable space until one of the termination conditions is met. Incoming characters are buffered in ring memory, the size of which is determined by the [SerialOpen](serialopen.md) function. The buffer can be cleared using the [SerialFlush](serialflush.md) instruction. For incoming data, extended ASCII characters (128-255) are supported only by SerialOpen format options 1-7 and 17-23.

## Parameters

# Dest (Destination)

The variable in which the incoming data will be stored. The variable is formatted as a string.

# ComPort_Serial_In (Communications Port Serial In)

The COM port that will be used by the instruction.Note thatbeginning with OS 6.0,control ports C1 and C2 can be configured individually for transmit (Tx) or receive (Rx).Right-click to display a list.

| Alphanumeric | Description                                |
| ------------ | ------------------------------------------ |
| ComUSB       | USB port of the datalogger                 |
| ComRS232     | RS232 port of the datalogger               |
| Com1         | Datalogger's control ports 1 (TX) & 2 (RX) |
| ComC1_Rx     | Configures control port 1 for receive only |
| ComC2_Rx     | Configures control port 2 for receive only |
| ComRF        | Integrated RF communication                |

Type: Constant. For all instructions except SerialOpen, this parameter can also be a variable. However, if a variable is used, the port must be opened previously by [SerialOpen (Open Communication Port)](serialopen.md)

A variable can be used in the ComPort parameter for use with functions that return a communication port. If communication occurs using TCP/IP, enter the variable for the socket returned by [TCPOpen](tcpopen.md).

# TimeOut (Wait Time)

For the SerialIn instruction, the TimeOut parameter is used to specify the amount of time, in 0.01 seconds, that the datalogger should wait before proceeding to the next instruction. If a 0 is entered for this parameter, the datalogger will wait for the termination character (TerminationChar parameter) or maximum number of characters (MaxNumChars parameter) to be received before proceeding. TimeOut is reset with each new incoming character.

For the SerialOut instruction, the TimeOut parameter is used to specify the amount of time, in 0.01 seconds, that the datalogger should wait for the WaitString or echo of each character in the OutString. If the TimeOut is 0, the datalogger does not wait (or even check) for the WaitString or Echo before proceeding to the next instruction. The TimeOut applies to each of the NumberTries; the overall timeout period for the instruction is cumulative. TimeOut is reset with each new incoming character.

As a result of internal buffering in the datalogger and/or external interfaces, data may not appear in the serial port buffer for a period of up to 50 ms (depending on the serial port being used). If the TimeOut is set at too short of an interval, the instruction may time out even though data has been received in the internal buffer (though not yet passed on to the serial port buffer). This should be taken into consideration when setting the TimeOut parameter.

Type: Constant

# TerminationChar (Termination Character)

Specifies a single character that marks the end of the incoming block of data. The character can be entered as anASCII

**Note:** Abbreviation for American Standard Code for Information Interchange / American National Standards Institute. An encoding scheme in which numbers from 0-127 (ASCII) or 0-255 (ANSI) are used to represent pre-defined alphanumeric characters. Each number is usually stored and transmitted as 8 binary digits (8 bits), resulting in 1 byte of storage per character of text.

character code or as a string. The termination character can be included in, or excluded from, the result string.[For more information, seeASCII Codes and Characters.](../Info/CodesChar.md)

**Exclude TerminationChar **: If the TerminationChar is an ASCII value between 0 and 255, it will terminate the string input upon seeing the character. The character is not included in the result string. To enter the termination character, simply use the [ASCII code](../Info/CodesChar.md).

** Include TerminationChar**: If the TerminationChar is declared as a string, the input string will terminate after a match of the termination string is found. The matching termination string will be included in the resultant string. For printable characters, the string can be entered directly (for instance, for a period character, enter . ). For non-printable characters, use the CHR() instruction along with the [ASCII code](../Info/CodesChar.md). As an example, for a carriage return (CR), enter CHR(13).

Entering a negative number or a null for the TerminationChar means there is no termination character.

Type: Integer, Variable, or Constant

# MaxNumChars (Maximum Number of Characters)

Specifies the maximum number of characters to expect per input. Once the maximum number of characters is received, the datalogger will proceed to the next instruction.

Type: Integer, Variable, or Constant
