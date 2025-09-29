# TCClockOption

The ClockOption parameter behaves somewhat differently depending upon whether the TimedControl instruction is placed outside of the program scan or within the program scan.

When the TimedControl instruction occurs prior to BeginProg, this option is used to set how the instruction will behave when the datalogger's clock is reset. When the TimedControl instruction is used within the program to reset or change the sequence, this option is used to set what happens between the time the instruction is executed and the SyncInterval occurs. Only option codes 1 and 2 are valid for this latter case. Right-click the parameter to display a list box of options.

| Option | Description                                                                                                                                  |
| ------ | -------------------------------------------------------------------------------------------------------------------------------------------- |
| 1      | The instruction behaves as if it were just started after compile, and the value sent to the SDMCD16 is the DefaultValue.                     |
| 2      | The sequence continues running as if nothing happened until the next occurrence of the SyncInterval, at which time it restarts.              |
| 3      | The change in the clock is ignored, and the TimedControl instruction proceeds with the CurrentIndex and duration as if nothing has happened. |

Type: Constant
