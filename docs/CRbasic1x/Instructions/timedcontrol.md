# TimedControl (Timed Control from SDM-CD16AC)

The TimedControl instruction is used only with an [SDM-CD16AC](sdmcd16ac.md) relay control port device. The TimedControl instruction allows a sequence of fixed values and durations to be controlled by the SDM task sequencer, thus allowing a control event to occur at a precise time, even if processing is lagging behind measurements. Note that programs using the TimedControl instruciton must run in [PipelineMode](sequentialmodepipeli2.md).

## Syntax

TimedControl(TCSize,TCSyncInterval,TCIntervalUnits,TCDefaultValue,TCCurrentIndex,TCSource,TCClockOption)

The following program uses the TimedControl instruction to precisely control the timing of ports on an SDM-CD16AC. Port 1 will be set high for 10 seconds, port 2 for 20 seconds, and ports 3, 4, and 5 for 30 seconds each. This sequence of port settings occurs every two minutes.

One TimedControl instruction is used outside the scan to initially set up the device, another is used within the scan to set up the port control sequence, and another is used within the scan to allow manual switching of the ports if needed.

```
PipeLineMode

'***Constants***
Const SCAN_INTERVAL = 100'100 mSec

'Switching Constants
Const NumSitesInSeq = 5
Const Scans_Sec = 1000/SCAN_INTERVAL

'Variables.
'The ValveNum Variable is used to Display the active Valve/SDMCD16 Channel
Public ValveNum As Long

'The ValveSet Array is used to set the timing for the control of the SDMCD16.
Public ValveSet(NumSitesInSeq,2) As Long
Public ValveSwitches(5) As Long
Public Index As Long

'Flags to Allow manual switching
Public SeqActiveFlag As Boolean
Public SeqProgramedFlag As Boolean

'Initial Setup of TimedControl sequence
TimedControl(NumSitesInSeq,2,Min,1,Index,ValveSet,2)

BeginProg
'Load SDMCD16 Settings for each valve number
'These values are used for manual control
ValveSwitches(1) = 2^0'2^0=1 or binary 1, Valve Number 1, SDMCD16 Channel 1'
ValveSwitches(2) = 2^1'2^1=2 or binary 10, Valve Number 2, SDMCD16 Channel 2
ValveSwitches(3) = 2^2'2^2=4 or binary 100, Valve Number 3, SDMCD16 Channel 3
ValveSwitches(4) = 2^3'2^3=8 or binary 1000, Valve Number 4, SDMCD16 Channel 4
ValveSwitches(5) = 2^4'2^4=16 or binary 10000, Valve Number 5, SDMCD16 Channel 5

'Load sequence array with valve setings
ValveSet(1,1) = 2^0'Valve Number 1, SDMCD16 Channel 1
ValveSet(2,1) = 2^1'Valve Number 2, SDMCD16 Channel 2
ValveSet(3,1) = 2^2'Valve Number 3, SDMCD16 Channel 3
ValveSet(4,1) = 2^3'Valve Number 4, SDMCD16 Channel 4
ValveSet(5,1) = 2^4'Valve Number 5, SDMCD16 Channel 5

'Load sequence array with site timing information,
'The equation allows the values to be entered as seconds rather than counts.
ValveSet(1,2) = Scans_Sec*10'10 Seconds on Site 1
ValveSet(2,2) = Scans_Sec*20'20 Seconds on Site 1
ValveSet(3,2) = Scans_Sec*30'30 Seconds on Site 1
ValveSet(4,2) = Scans_Sec*30'30 Seconds on Site 1
ValveSet(5,2) = Scans_Sec*30'30 Seconds on Site 1
'Note that the total number of seconds adds up to 2 minutes

SeqActiveFlag = True

Scan (SCAN_INTERVAL,mSec,10,0)
'Measure Sensors
'(sensor measurements omitted from program)
If SeqActiveFlag Then
ValveNum=Index
If SeqProgramedFlag = False Then
ValveSet(1,1) = ValveSwitches(1)
ValveSet(1,2) = Scans_Sec*10
TimedControl(NumSitesInSeq,2,Min,1,Index,ValveSet,2)
SeqProgramedFlag = True
EndIf
Else
ValveSet(1,1) = ValveSwitches(ValveNum)
ValveSet(1,2) = 0
If SeqProgramedFlag = True Then
TimedControl(1,0,Min,ValveSwitches(ValveNum),Index,ValveSet,1)
SeqProgramedFlag = False
EndIf
EndIf
'Set the SDMCD16AC
SDMCD16AC (ValveSet,1,1)
NextScan
EndProg
```

## Remarks

As currently implemented, the TimedControl instruction allows a series of [SDM-CD16AC](sdmcd16ac.md) settings controling valves, to be defined so that the switching sequence occurs at precise times, even if processing is lagging behind measurements. This is accomplished by running the instruction in the SDM task sequencer rather than in the processing task sequencer, thus assigning the task a higher priority of execution in the datalogger.

The TimedControl instruction must be placed before BeginProg to declare the use of the timed control. If it is necessary to change the sequence while the program is running, additional TimedControl instructions can be placed within the program.

## Parameters

# TCSize

Defines the number of values in the sequence.

Type: Constant

# TCSyncInterval

The interval on which to run the TimedControl sequence. When the program is compiled and starts running or when TimedControl is reset, the program will wait until an even multiple of the SyncInterval to start the sequence. Enter 0 to start immediately.

The ClockOption parameter determines whether the SyncInterval will be restarted or remain unchanged when the datalogger's clock is changed or the instruction is reset.

Type: Constant or Variable declared as Type Long

# TCIntervalUnits

The unit of measure to use for the SyncInterval. Valid options are microseconds, milliseconds, seconds, minutes, hours, or days.

Type: Constant

# TCDefaultValue

A binary value sent to the SDMCD16 prior to the beginning of the TimedControl sequence or when ClockOption 1 is selected and the clock is reset.

Type: Variable or Constant

# TCCurrentIndex

A variable that holds the array index that indicates which control sequence is being executed for the TimedControl instruction. The CurrentIndex is set to 0 when TimedControl is reset or is resyncing.

Type: Variable

# TCSource

A two-dimensional array that holds a binary value to be sent to the SDMCD16 to determine the port to be set and the duration (number of scans) the value should be used. For Array(x,y), x is the binary value determining the port and y is the number of scans. X should be dimensioned to at least the size of the Size parameter.

Type: Two dimensional Variable Array.

# TCClockOption

The ClockOption parameter behaves somewhat differently depending upon whether the TimedControl instruction is placed outside of the program scan or within the program scan.

When the TimedControl instruction occurs prior to BeginProg, this option is used to set how the instruction will behave when the datalogger's clock is reset. When the TimedControl instruction is used within the program to reset or change the sequence, this option is used to set what happens between the time the instruction is executed and the SyncInterval occurs. Only option codes 1 and 2 are valid for this latter case. Right-click the parameter to display a list box of options.

| Option | Description                                                                                                                                  |
| ------ | -------------------------------------------------------------------------------------------------------------------------------------------- |
| 1      | The instruction behaves as if it were just started after compile, and the value sent to the SDMCD16 is the DefaultValue.                     |
| 2      | The sequence continues running as if nothing happened until the next occurrence of the SyncInterval, at which time it restarts.              |
| 3      | The change in the clock is ignored, and the TimedControl instruction proceeds with the CurrentIndex and duration as if nothing has happened. |

Type: Constant
