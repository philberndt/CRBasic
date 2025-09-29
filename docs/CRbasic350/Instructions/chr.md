# CHR (Character)

The CHR string function returns a character in the extended ASCII character set.

## Syntax

CHR(Code)

The following program shows the use of the CHR instruction in a string. When an alarm condition is met (variable Indoor is greater than variable Outdoor by 5), the text string "Warning Indoor Temp > Safe Level" is printed in the output table. The CHR instruction is used to produce the > character.

```
Public RefTemp, TCTemp(2)
Alias TCTemp(1) = Indoor
Alias TCTemp(2) = Outdoor
Dim AlarmText As String * 50

DataTable (Alarm,1,-1)
DataInterval (0,1,Sec,10)
Sample (1,AlarmText,String)
Sample (1,Indoor,FP2)
Sample (1,Outdoor,FP2)
EndTable

BeginProg
Scan (1,Sec,3,0)
PanelTemp (RefTemp,60)

TCDiff (TCTemp(),2,mv34,1,TypeT,RefTemp,False,0,4000,1.0,0)
'Evaluate temperature
If Indoor - Outdoor > 5 Then
'If Difference > 5 then output warning text
AlarmText = "Warning Indoor Temp " +CHR(155)+ " Safe Level"
'Otherwise, output an empty string
Else
AlarmText = ""
EndIf
CallTable Alarm
NextScan
EndProg
```

Following is an example of the data file produced from this program:

"TOA5","CR300Series","CR300","1010","CR300.Std.00.43","CPU:chr.CR300","47476","Alarm"

"TIMESTAMP","REC OR D","AlarmText","Indoor","Outdoor"

"TS","RN","","",""

"","","Smp","Smp","Smp"

"2004-09-07 14:10:24",29,"",24.75,24.83

"2004-09-07 14:10:25",30,"",24.81,24.84

"2004-09-07 14:10:26",31,"",25.89,24.84

"2004-09-07 14:10:27",32,"Warning Indoor Temp Safe Level",30.21,24.88

"2004-09-07 14:10:28",33,"Warning Indoor Temp Safe Level",31.36,24.88

"2004-09-07 14:10:29",34,"Warning Indoor Temp Safe Level",31.69,24.92

"2004-09-07 14:10:30",35,"Warning Indoor Temp Safe Level",30.3,24.92

"2004-09-07 14:10:31",36,"",29.56,24.92

"2004-09-07 14:10:32",37,"",29.07,24.94

"2004-09-07 14:10:33",38,"",28.64,24.94

"2004-09-07 14:10:34",39,"",28.3,24.93

"2004-09-07 14:10:35",40,"",28.03,24.9

"2004-09-07 14:10:36",41,"",27.82,24.96

"2004-09-07 14:10:37",42,"",27.53,24.97

"2004-09-07 14:10:38",43,"",27.31,24.97

"2004-09-07 14:10:39",44,"",27.07,24.97

The following program creates a string of &h0A0B0C00 and sends it out Com1. This Demonstrates using the CHR() function to create strings of unprintable ASCII.

```
Public OutputString As String * 16
BeginProg
SerialOpen (COM1,9600,3,0,50)
Scan (1,Sec,0,0)
'OutputString(1,1,1) = CHR(&h0A)
'OutputString (1,1,3) = CHR(&h0C)
'OutputString (1,1,2) = CHR(&h0B)
'OutputString now contains &h0A0B0000

'Must build the string in order
OutputString(1,1,1) = CHR(&h0A)
OutputString (1,1,2) = CHR(&h0B)
OutputString (1,1,3) = CHR(&h0C)
'OutputString now contains &h0A0B0C00
SerialOutBlock (COM1,OutputString,4)
NextScan
EndProg
```

## Remarks

The CHR function returns a string consisting of the character indicated followed by a null character. CHR(0) will produce a string of two nulls. The character returned by the CHR function can be stored in a string in the program or sent to some other device by using serial commands (for example, SerialOut).

**NOTE:** Characters up to 127 represent the standard ASCII character set and will mostly likely work correctly in any application. Characters beyond 127 are dependent on the application using the created string.

## Parameter

# Code

The code for the character to be output. The Code parameter is the integer value (0 through 255) that is the extended ASCII representation of the character to output. See [ASCII Codes and Characters](../Info/CodesChar.md) for a full list of codes and their associated characters.

Type: Integer, 0 to 255

String variables can be declared as only one or two dimensions; for example, String(x) or String(x,y). To begin reading or modifying a string at a particular location into the string, enter the location as a third dimension; for example, String(x,y,n) where n is the desired character. For example, given an array of strings Str(10,10), Str(2,2,n) refers to n character in the (2,2) element of the array. Use Str(1,1,n) for a scalar variable and Str(x,1,n) for a one dimensional array element.
