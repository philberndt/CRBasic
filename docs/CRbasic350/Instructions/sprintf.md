# Sprintf (Format Output String)

The Sprintf function is used to write a formatted output string to a destination variable. It works like sprintf in C and is especially helpful for formatting output, creating dynamic filenames, generating status messages, or customizing log entries.

## Syntax

_Length_ = Sprintf(Dest,Format,Argument1..Argument10)

### Example #1

The following program uses SprintF to format a string and send it out the RS-232 port. This could be used to send sensor data to a serial LCD.

The two measurement instructions may need to be modified for other datalogger types.

The output string from this program can be seen by connecting a cable to the datalogger's RS-232 port and using a terminal emulator (such as in Device Configuration Utility).

```
Public RefTemp, TCTemp
Public OutputString As String * 50

BeginProg
'Open Serial port for communication
SerialOpen (COMRS232,115200,0,0,50)

Scan (5,Sec,3,0)
PanelTemp (RefTemp,4000)
TCDiff (TCTemp,1,mv34,1,TypeT,RefTemp,True,0,4000,1.0,0)

'Generate string using SprintF
'%f is used to format variable TCTemp
'%c (formatting a character code of 10) outputs a line feed
SprintF( OutputString, "The current air temperature is %3.2f %c", TCTemp,10)

SerialOut (COMRS232,OutputString,"",0,0)

NextScan
EndProg
```

### Example #2

The following program writes readable, customized logs to a text file called CustomLog.Txt on the dataloggerCPUdrive. The log file contains the battery voltage with 2 decimal places and the panel temp with 1 decimal place.

```
Public BattVolt, PTemp
Public OpenFile1 As Long, CloseStat
Public FileStr As String * 100

BeginProg

Scan (60,Sec,0,0)
PanelTemp (PTemp,4000)

Battery (BattVolt)
'Generate string using SprintF
'%f is used to format variables BattVolt and PTemp
'%c (formatting a character code of 10) outputs a line feed
Sprintf(FileStr, "Battery: %.2f V, Panel: %.1f C %c", BattVolt, PTemp, 10)
OpenFile1 = FileOpen ("CPU:CustomLog.txt","a",-1)
FileWrite(OpenFile1, FileStr,0)
CloseStat=FileClose (OpenFile1)
NextScan
EndProg
```

### Example #3

The following program writes a daily log file to the dataloggerCPUdrive with a dynamic filename based on date. In this example, replace "Logged Data Line" with what you would like to write to the log file.

```
Public BattVolt, PTemp
Public OpenFile1 As Long, CloseStat
Public FileName As String * 100
Public DLTime(9) as Long

BeginProg
Scan (60,Sec,0,0)
PanelTemp (PTemp,4000)

Battery (BattVolt)
RealTime(DLTime)
'Generate output filename using SprintF
'%d is used to format variables of the year, month, and day of the from the RealTime instruction
Sprintf(FileName, "CPU:Log_%04u%02u%02u.txt", DLTime(1), DLTime(2), DLTime(3))
OpenFile1 = FileOpen (FileName,"a",-1)
FileWrite(OpenFile1, "Logged Data Line",0)
FileWrite (OpenFile1,CHR(13),0)
FileWrite (OpenFile1,CHR(10),0)
CloseStat=FileClose (OpenFile1)
NextScan
EndProg
```

### Example #4

The following program uses Sprintf to compose a status email message with formatted sensor values.

```
Public BattVolt, PTemp

'EmailRelay() variables
Public Subject As String * 50 = "SiteStatus"
Public Message As String * 200
Public EmailSuccess
Public ServerResp As String * 100

Const ToAddr As String = "name@gmail.com"

BeginProg
Scan (60,Sec,0,0)
PanelTemp (PTemp,4000)

Battery (BattVolt)
'Generate string using SprintF
'%f is used to format variables BattVolt and PTemp
'%c (formatting a character code of 10) outputs a line feed
Sprintf(Message, "Battery: %.2f V, Panel: %.1f C", BattVolt, PTemp)
EmailSuccess =EmailRelay(ToAddr,Subject,Message,ServerResp)
NextScan
EndProg
```

## Remarks

Sprintf is used to format an output string and store it in a destination variable. It allows you to create well-structured strings by specifying format specifiers for different data types, such as integers, floating-point numbers, and hexadecimal values. For example, you can use Sprintf to format a floating-point number with a specific precision or to construct a message for output via an RS-232 port. The Sprintf function returns the length of the string written to the destination variable (Dest). Up to 10 arguments can be passed into the Format string, with format specifiers corresponding one-to-one with the arguments. The function requires an argument for each format specifier. Any arguments not paired with a format specifier are discarded.

## Parameters

# Dest (Destination)

Variable in which to store the formatted output string. Dest should be sized large enough to accommodate all of the characters written to the resulting string. No error is returned if Dest is sized too small.

Type: String variable

# Format

A string used to define the format to be used for the arguments. Up to 10 format specifiers (with corresponding arguments) can be used in a format string. The format specifiers are:

| Specifier | Description                                                             |
| --------- | ----------------------------------------------------------------------- |
| d or i    | Signed decimal integer                                                  |
| u         | Unsigned decimal integer                                                |
| o         | Unsigned octal                                                          |
| b         | Binary (base 2)                                                         |
| x         | Unsigned hexadecimal integer                                            |
| X         | Unsigned hexadecimal integer (uppercase)                                |
| f         | Decimal floating point, lowercase                                       |
| e         | Scientific notation (mantissa/exponent), lowercase                      |
| E         | Scientific notation (mantissa/exponent), uppercase                      |
| g         | Use the shortest representation: %e or %f                               |
| G         | Use the shortest representation: %E or %F                               |
| c         | Single character                                                        |
| s         | String of characters                                                    |
| y (or Y)  | SDI-12 compatible output. INF and NAN are output as 9999999             |
| z (or Z)  | %m.nZ fits n significant digits into a width of m characters            |
| %         | A % followed by another % character will write a single % to the stream |

**NOTE:** If L is specified before f, e, or g, then the value passed in is of type [double](../Info/DoublePrecArith.md) and, therefore, the precision of double can be displayed. The maximum number of digits possible is 7 for floats (IEEE4, single precision) and 18 for doubles(IEEE8, double precision).

Width and precision may be specified directly in the format string or may be passed in from one of the arguments. When specifying width and/or precision as an argument their place should be indicated by an asterisks (\*). The following examples are equivalent and will return a floating point value with a field width of at least 3 characters and a precision of 2 characters.

sprintf(dest,"%3.2f",67.89)

sprintf(dest,"%\*.2f",3,67.89)

sprintf(dest,"%\*.\* f",3,2,67.89)

Precision for f specifies the number of digits after the decimal point, whereas precision for g and e specifies the total number of digits displayed, including digits before and after the decimal point.

For example:

sprintf(Str(6),"%.10Lf",112.000009R) prints 112.0000090000

sprintf(Str(8),"%.10Lg",112.000009R) prints 112.000009

Up to five Flags can be used immediately following the % sign:

| Flag  | Description                                                                                                                                                           |
| ----- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| +     | Always denote the sign of a number                                                                                                                                    |
| Space | Prefixes a non-negative signed number with a space                                                                                                                    |
| -     | Left-align the output of the placeholder (default is to right-align)                                                                                                  |
| #     | For g and G, trailing zeros are not removed. For f, e, g, or G, the output always contains a decimal point. For o, x, and X, prepend o, Ox, or OX to non-zero numbers |
| 0     | Use 0 spaces to pad a field when the width option is used                                                                                                             |

Type: Variable declared as a string

# Argument1..Argument10

The arguments to pass into the format string. Up to 10 arguments can be entered. These arguments can be decimal values, integers, strings, variables, or expressions. The data type of these arguments must match the format specifier. **Usage notes:** Sprintf performs no data type conversions on the arguments passed into the format string. The format specifier must match the argument type or erroneous values are returned.

[TableName.Timestamp](tablenametimestamp.md) by default is returned as an integer. To print a timestamp it must be placed inside an expression that forces it to be converted to a string.

Arguments from a data table that are stored asFP2

** Note:**Two-byte floating-point data type. Default datalogger data type for stored data. While IEEE four-byte floating point is used for variables and internal calculations, FP2 is adequate for most stored data. FP2 provides three or four significant digits of resolution, and requires half the memory as IEEE4.

are converted to floats before they are accessed. The precision for these values can be represented fairly closely by using %4.g, though if the mantissa is <1000 it is only 3 significant digits (i.e., %.3g).

For all options but y (or Y), infinity is output asINF

** Note:**A data word indicating the result of a function is infinite or undefined.

and not a numberNAN

** Note:**Not a number. A data word indicating a measurement or processing error. Voltage overrange, SDI-12 sensor error, and undefined mathematical results can produce NAN.

.

A formatted value can be padded with zeros (or spaces), using the notation %#code (right justified) or %-#code (left justified), where # is the number of spaces for the field. Consider the following examples, where the Long variable is 12345.

If FormatString = %8d the result is (space)(space)(space)12345

If FormatString = %-8d the result is 12345(space)(space)(space)

If FormatString = %08d the result is 00012345

If FormatString = %-08d the result is 12345000
