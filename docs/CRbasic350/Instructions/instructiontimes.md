# InstructionTimes (Instruction Processing Time)

The InstructionTimes instruction returns the processing time of each instruction in the program.

## Syntax

InstructionTimes(Dest)

### Example #1

The following program measures battery voltage, panel temperature, and a thermocouple. The InstructionTimes instruction is used to return the time of processing for each instruction. There are 20 lines in the program, so the Destination array for InstructionTimes is dimensioned to 20.

This program was written for aCR350, but other dataloggers can use similar code (voltage ranges, channel numbers, or other parameters may need to be changed to reflect the specifications of the datalogger).

```
'CR350Datalogger

Public BattVolt, PTemp, TCTemp, ITimes(20) AS LONG
InstructionTimes(ITimes())

DataTable (TempTbl,1,-1)
DataInterval (0,1,Min,10)
Sample (1,BattVolt,FP2)
Sample (1,PTemp,FP2)
Sample (1,TCTemp,FP2)
EndTable

BeginProg
Scan (1,Sec,3,0)
Battery (BattVolt)
PanelTemp (PTemp,4000)
TCDiff (TCTemp,1,mv34,1,TypeT,PTemp,True,0,4000,1.0,0)

CallTable TempTbl
NextScan
EndProg
```

### Example #2

The following program demonstrates using InstructionTimes to debug a program that is skipping scans. For demonstration, run this program and compare the line that the Scan() instruction is on to the line that SerialIn() is on. Any instruction that has a greater processing time than the scan instruction will result in skipped scans. In this case, the processing time for the entire scan is about 1.6 seconds, while the processing time for Serialin is > 5 sec. Hence, SerialIn is causing the skipped scans. To remove the skipped scans we could either decrease the delay specified in the Serialin instruction, or increase the scan time.

```
'CR350Datalogger

Public RefTemp, batt_volt
Public Counter
Public OutString As String * 1000
Public InString As String * 1000
Public ITimes(45)As Long
Public SkipScans As Long

InstructionTimes(ITimes())

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
'Jumper wire from C1 (ComC1_Tx) to C2 (ComC2_Rx)

SerialOpen (ComC1_Tx,115200,0,0,10000)
SerialOpen (ComC2_Rx,115200,0,0,10000)

Scan (1,Sec,0,0)

PanelTemp (RefTemp,4000)
Battery (batt_volt)
SkipScans = Status.SkippedScan'Pull the number of skipped scans from the Status Table.
counter = counter + 1
OutString = "enter output string here " + Counter
'Send String over control port C1 (ComC1_Tx).
SerialOut (ComC1_Tx,OutString,"",0,100)
'Receive String over control portC2 (ComC2_Rx).
SerialIn (InString,ComC2_Rx,500,0,1000)'500 = 5 sec delay.

'Call Output Tables
CallTable Test

NextScan
EndProg
```

## Remarks

The InstructionTimes instruction loads the Dest array with processing times for each instruction in the program. The time is reflected in microseconds. InstructionTimes must appear before the BeginProg statement in the program.

Each element in the array corresponds to a line number in the program. To accommodate all of the instructions in the program, the array must be dimensioned to the total number of lines in the program, including blank lines and comments. The Dest array must also be dimensioned as a long integer (for example, Public Array(20) AS LONG).

**NOTE:** The processing time for an instruction may vary. For instance, it will take longer to execute instructions when the datalogger is communicating with another device.

**NOTE:** The InstructionTimes() instruction will not output while a Debug() instruction is in the data logger program. When Debug() is in use, instruction execution times can be viewed in the trace output of Debug().

### Troubleshooting Variable Out Of Bounds Error

InstructionTimes can be inserted into a program that is returning a variable out of bounds error to indicate which variable is in error. A variable out of bounds error, along with the line number of the instruction that is causing the problem, will show up in the [Compile Results](../Info/compilemenu.md) of the datalogger.

A variable out of bounds error can occur at run-time if an instruction attempts to write beyond the size of a dimensioned array (for instance, the instruction tries to write to Array(5), but Array is declared as Array(4)).
