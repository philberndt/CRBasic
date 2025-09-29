# Debug, DebugBreak

The Debug/DebugBreak instructions are used to set breakpoints in program code to help troubleshoot program execution

## Syntax

Debug(DebugSequence,HistorySize,Control,LineBreak,TraceHistory)

DebugBreak

The following example program shows the use of the Debug instruction. The debug control variable, DeControl, is initially set to 0, thus, the program will execute normally until a different control value is entered into the DeControl variable. The first break is set just after the scan. Additional breaks can be set in the program by entering line numbers into the DeBreaks array.

```
Public PTemp, TCTemp(3)
Units TCTemp() = Deg C
Public DeBreaks(5) As Long, DeControl As Long, DeTrace As String * 100000

Debug(-1,20,DeControl,DeBreaks(),DeTrace)

'Define output tables
DataTable (DebugTable,True,-1 )
Sample (1,DeControl,FP2)
Sample (1,DeTrace,String)
EndTable

'Main Program
BeginProg
DeControl = 0
Scan (2,Sec,3,0)
DebugBreak
PanelTemp (PTemp,15000)
TCDiff (TCTemp(),3,mv200C,1,TypeT,PTemp,True,0,15000,1.0,0)

CallTable (DebugTable)
NextScan
EndProg
```

## Remarks

The Debug instruction is a declaration which has no runtime code. It must appear in the program prior to any DebugBreak instructions. The DebugBreak instruction stops execution of the program on the line where it appears. When a break is encountered program execution will remain stopped until the Control variable is set to a value that resumes execution of the program.

The LineBreaks array shows the line numbers of the break points in the program. These values can be changed during program execution to edit break points.

## Parameters

# DebugSequence

Controls which program sequences (program scans) will be included in the DebugTrace information. Valid options are:

| Code | Description                         |
| ---- | ----------------------------------- |
| -1   | Trace all sequences                 |
| 0    | Trace only main sequence            |
| 1    | Trace only the first slow sequence  |
| 2    | Trace only the second slow sequence |
| 3    | Trace only the third slow sequence  |
| 4    | Trace only the fourth slow sequence |

Type: Constant

# HistorySize

Sets the number of lines that will be displayed in the TraceHistory.

Type: Constant Integer

# Control

A variable declared as a long that controls whether or not the program breaks as DebugBreak points, and when and how it will resume after the break. The Control codes are:

| Code | Description                                                                            |
| ---- | -------------------------------------------------------------------------------------- |
| 0    | Program execution will proceed normally; no breaks                                     |
| -1   | Program execution will proceed normally until a break point is encountered             |
| 1    | Program execution will break immediately when Control is set to 1                      |
| 2    | Program execution will step to the next instruction                                    |
| 3    | Program execution will step to the next instruction, but over a Function or Subroutine |

Type: Variable declared as Long

# LineBreak

A variable declared as a long that contains the line numbers for the break points. These line numbers are set by the DebugBreak instructions in the program, but they can also be changed during program run-time.

Type: Variable declared as Long

# TraceHistory

A variable declared as a string that holds the program execution trace history preceding the break point. Each line in the trace history displays a timestamp, execution time in microseconds, program sequence number (0 = main scan, 1 through 4 are the slow sequences), line number, and the line of code in the program file.

Type: Variable declared as a String
