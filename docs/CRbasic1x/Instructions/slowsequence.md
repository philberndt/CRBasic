# SlowSequence (Execute Instructions at Different Rate)

The SlowSequence instruction allows one or more instructions to be executed at a different rate than that of the main program.

## Syntax

SlowSequence

EndSequence _'optional_

The example uses SlowSequence to calibrate the datalogger every sixty seconds.

```
'Calibrate
Public Temp1
Public Calib1(60)
DataTable(Table1,1,600)
DataInterval(0,1,sec,1)
Sample(1,Temp1,FP2)
EndTable

BeginProg
Scan(100,mSec,0,0)
PanelTemp (Temp1,60)
CallTable Table1
NextScan

SlowSequence
Scan (60,Sec,0,0)
Calibrate(Calib1)
NextScan

EndProg
```

## Remarks

The SlowSequence statement marks the end of the main program and begins a separate sequence of instructions. The instructions for the slow sequence program are executed when the main program is not running as time allows. It is possible to have up to four slow sequences executing at a rate different than that of the primary scan interval. If multiple sequences are desired, each SlowSequence in the program must be proceeded by a SlowSequence statement. Slow sequences can be declared within a Scan/NextScan structure, or they can be placed within a Do/Loop to execute whenever the datalogger is not busy with other tasks. The execution of slow sequences is most often controlled by the Scan instruction.

**NOTE:** The BufferOption parameter in the Scan instruction is ignored in a slow sequence. Measurements made in a slow sequence are stored in a single buffer, and processing of this buffer is completed before the NextScan measurements are made.

The EndSequence instruction marks the end of a sequence that started with BeginProg, a SlowSequence declaration, or a TriggerSequence, and ends any accompanying declaration sequences (such as [DataTable](datatable.md) declarations). Use of EndSequence prevents insertion of a declaration sequence in the middle of some other executing sequence of code. Declaration sequences must occur before BeginProg, after an EndSequence but before a subsequent SlowSequence, or immediately following a SlowSequence declaration.

The main scan has priority over all other sequences. The instructions in one or more slow sequence are performed, in the order they appear in the program, during the times when the datalogger is not running in the main scan. If the time arrives for the main scan to run while a slow sequence scan is in progress with a measurement, the measurement in the slow sequence is finished first, and then the main scan is run.

See [Sequential and pipeline processing modes](https://help.campbellsci.com/CR1000X/Content/shared/Details/PipelineSequential.htm) for information on SlowSequence priorities when running in pipeline mode vs sequential mode.

Slow sequences are typically run at a slower rate than the main program. They can be run at a faster rate; however, there is a high risk of skipping scans in the slow sequence if the main scan interval is set too fast or if the interval for the SlowSequence and the main scan occur simultaneously.

Data written to data tables within a slow sequence are time stamped with the start time of the slow sequence scan.
