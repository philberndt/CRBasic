# Scan, NextScan (Set Scan Rate, Proceed to Next Scan)

The Scan instruction is used to establish the program scan rate. The NextScan instruction shifts program control to the Scan instruction.

## Syntax

```
Scan
```

(Interval,Units,BufferOption,Count)

```
ExitScan
```

ContinueScan

```
NextScan
```

The following examples use Scan and NextScan in a simple program.

### Example #1

```
BeginProg
Scan( 1, sec, 1, 0 )'Continuous Scan/Next Scan Loop
'(measurement instructions, calls to data tables, etc.)
NextScan
EndProg
```

### Example #2

```
BeginProg
Scan( 1, sec, 1, 2000 )'Make only 2000 scans then stop
'(measurement instructions, calls to data tables, etc.)
NextScan
EndProg
```

### Example #3

The following, more advanced, program shows the use of ExitScan to transition from one scan to the next, and the use of a loop structure to loop a series of stacked scans.

```
'counters for demonstration
Public Scan1, Scan2, Scan3, Scan4
'flags for triggering execution of ExitScan statements
Public Flag(4) As Boolean

BeginProg'begin program execution

'Scan Loop #1
Scan (1,Sec,0,0)'loop / scan forever
If Flag(1) Then ExitScan'if flag(1) is set high, exit this scan loop
Scan1 = Scan1 + 1'increment counter for visual feedback
NextScan

'can perform operations during transition from one scan loop to the next
Flag(1) = FALSE'unset flag(1)

'Scan Loop #2
Scan (1,Sec,0,0)'loop / scan forever
If Flag(2) Then ExitScan'if flag(2) is set high, exit this scan loop
Scan2 = Scan2 + 1'increment counter for visual feedback
NextScan

Flag(2) = FALSE'unset flag(2)

Do'enter an infinite loop, do the following

'Scan Loop #3
Scan (1,Sec,0,0)'loop / scan forever
If Flag(3) Then ExitScan'if flag(3) is set high, exit this scan loop
Scan3 = Scan3 + 1'increment counter for visual feedback
NextScan

Flag(3) = FALSE'unset flag(3)

'Scan Loop #4
Scan (1,Sec,0,0)'loop / scan forever
If Flag(4) Then ExitScan'if flag(4) is set high, exit this scan loop
Scan4 = Scan4 + 1'increment counter for visual feedback
NextScan

Flag(4) = FALSE'unset flag(4)

Loop'loop back to do and start over with scan loop #3

EndProg'end program execution
```

## Remarks

The measurements, processing, and calls to output tables bracketed by the Scan NextScan instructions determine the sequence and timing of the datalogger program.

The Scan instruction determines how frequently the measurements within the Scan NextScan structure are made and sets the number of times to loop through the scan.

## Parameters

# Interval (Time Interval)

Enter the time interval on which the instructions between the Scan NextScan program structure should be executed. The interval may be in milliseconds, seconds, minutes, hours, or days, whichever is selected with the Units parameter. The minimum possible scan interval is 1 millisecond; maximum scan interval is one day. Resolution is 1 millisecond.

**NOTE:** The actual minimum scan interval will depend on the measurements and data storage tables in the program.

Type: Constant (or expression that evaluates as a constant)

# Units

The units for the interval parameter. Right-click the parameter to display a list.

| Code | Description  |
| ---- | ------------ |
| msec | milliseconds |
| sec  | seconds      |
| min  | minutes      |
| hr   | hours        |
| day  | day          |

Type: Constant

# BufferOption (Buffer Option)

This parameter is not used in the CR300 datalogger.

# Count

The number of times to execute the Scan/NextScan loop. A count of 0 will loop forever, or until the ExitScan condition is encountered.

Type: Integer

# ExitScan

The ExitScan statement can be used to specify a condition that will exit the Scan loop, regardless of the value of Count. Typically this is used to exit the current scan and pass control of the program to a subsequent scan.

# ContinueScan

The ContinueScan statement is used to jump to the NextScan statement without processing the remainder of the instructions in the scan. The datalogger then waits on the next Interval to resume.

# NextScan

The NextScan statement transfers program control to the Scan statement. It marks the end of the Scan NextScan control structure.
