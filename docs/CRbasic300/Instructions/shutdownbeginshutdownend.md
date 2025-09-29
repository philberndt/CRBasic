# ShutDownBegin/ShutDownEnd (Run Instruction Before Program Shutdown)

The ShutDownBegin/ShutDownEnd instructions are used to define one or more instructions that are run prior to the datalogger stopping execution of its program or compiling a program.

## Syntax

ShutDownBegin

_'instructions to run_

ShutDownEnd

The following program can be used to test the effects of various shut downs on a program using the ShutDownBegin/ShutDownEnd instructions.

When a shut down event occurs, a string is sent out the RS232 port. This program can be tested by connecting the datalogger's RS232 port to a computer's COM port, and then monitoring that port using a terminal emulator. When a shut down event occurs, a string is sent out the RS232 port (the string contains the current timestamp from the public table and a text string). You can use File Control (which connected via the CS I/O) or the datalogger's keyboard display to change the running program options.

Practical applications for this instruction might be setting a control port high so that a communications peripheral is enabled, closing down a communications connection (such as a serial port connection), or sending a command string to a device (such as AT commands to a modem).

```
Public BattVolt, Time As String * 25

ShutDownBegin
SerialOut (COMRS232,Time+" This is the outstring"+CHR(13)+CHR(10),"",0,100)
ShutDownEnd

BeginProg
SerialOpen (ComRS232,115200,0,0,10000)
Scan (1,Sec,3,0)
Battery (BattVolt)
Time=Public.TimeStamp(1,1)
NextScan
EndProg
```

## Remarks

The ShutDownBegin and ShutDownEnd instructions should be placed at the beginning of the program (or beginning of a [SlowSequence](slowsequence.md)) in the declarations section. The instructions that lie between ShutDownBegin and ShutDownEnd are executed when the currently running datalogger program is stopped under normal conditions (see following information on Shutdown events). This allows the datalogger to put itself or some peripheral (such as a modem) into a known state prior to stopping execution of its program (or prior to restarting/recompiling the current program).

Shutdown events include:

From File Control:

| - Clearing a program (**Stop Program | Clear the Program **) |
| - Restarting a program (** Run Options | Restart Program after Stop Program | Stop the Program **was previously selected) |
| - Applying the Run Now attribute to a different program on the datalogger (** Run Options | Run Now **) |

From Keyboard Display, Run/Stop Program options:

- Choosing** Stop **,** Delete Data **

- Choosing a** Restart **option after** Stop **,** Retain Data **

- Choosing a new program to** Run Now**

Under Program Control:

- Running a new program or stopping the program with the [FileManage](filemanage.md) instruction
