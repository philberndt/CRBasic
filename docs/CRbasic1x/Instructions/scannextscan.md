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
| μsec | microseconds |
| msec | milliseconds |
| sec  | seconds      |
| min  | minutes      |
| hr   | hours        |
| day  | day          |

Type: Constant

# BufferOption (Buffer Option)

Determines how data will be buffered during the Scan NextScan process (this option can be left at 0 if the datalogger is running in sequential mode since measurements and processing are carried out in sequential order rather than concurrently, and no processing tasks are buffered). The options are:

| Code       | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| ---------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 0, 1, or 2 | The datalogger uses two buffers when processing measurements. When a measurement begins on a scan, the values of the previous scan are loaded into a buffer. This allows processing to finish on the previous scan during measurement of the current scan. For CPI networks, beginning with datalogger OS4, setting the BuffOption to 2 will result in aCPI Frame Syncon every scan. This will force data to be processed immediately upon reception and will allow any control/output to take place immediately. |
| >3         | The datalogger uses three or more buffers when processing measurements, based on the number of scans defined by this Constant.                                                                                                                                                                                                                                                                                                                                                                                    |

Larger buffers can be used for a Scan that has occasional large processing requirements such as FFTs or Histograms, and/or when processing may be interrupted by communications. If a value of 1000 is inserted into the BufferOption argument of a scan having 10 thermocouple measurements, 40,000 bytes of SRAM will be allocated for the buffer [(4 bytes) / (measurement) x (10 measurements)/(buffered scan) x 1000 buffered scans)]. The buffer size plus the size of any Output Tables stored in SRAM should not exceed 4 megabytes.

If the processing ever lags behind by more than the buffer allocated, the datalogger will discard the buffered values and synchronize back up to the current measurement. The number of scans in the buffer waiting to be processed can be monitored by looking at the Buff.Depth parameter in the datalogger's status table. If the number of measurements waiting to be processed becomes greater than Buff.Depth can handle, the datalogger will "catch back up" and a Skipped Scan will be recorded.

The SlowSequence instruction does not allow for this buffering scheme even though Scan is used to signify the start of a scan in a slow sequence. In SlowSequence, the measurements are stored in a single buffer. Processing of this buffer is completed before the NextScan measurements are made.

Type: Constant

# Count

The number of times to execute the Scan/NextScan loop. A count of 0 will loop forever, or until the ExitScan condition is encountered.

Type: Integer

# ExitScan

The ExitScan statement can be used to specify a condition that will exit the Scan loop, regardless of the value of Count. Typically this is used to exit the current scan and pass control of the program to a subsequent scan.

# ContinueScan

The ContinueScan statement is used to jump to the NextScan statement without processing the remainder of the instructions in the scan. The datalogger then waits on the next Interval to resume.

# NextScan

The NextScan statement transfers program control to the Scan statement. It marks the end of the Scan NextScan control structure.
