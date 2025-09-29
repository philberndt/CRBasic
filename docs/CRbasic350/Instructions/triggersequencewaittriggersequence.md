# TriggerSequence, WaitTriggerSequence (Control Slow Sequence)

The TriggerSequence and WaitTriggerSequence instructions are used to control the execution of code within a [SlowSequence](slowsequence.md) scan.

## Syntax

Main program or SlowSequence

TriggerSequence(SequenceNum,TimeOut)

EndSequence(optional)

SlowSequence

WaitTriggerSequence

[code to execute when trigger occurs]

In the following program, the TriggerSequence/WaitTriggerSequence instructions are used to trigger the execution of the SendVariables instruction performing a datalogger call-back to the PC if temperature is greater than 80 degrees F.

This program was written for aCR350, but other dataloggers can use similar code (voltage ranges, channel numbers, or other parameters may need to be changed to reflect the specifications of the datalogger).

```
Public RefTemp, TCTemp, SendResult, Trigger As Boolean
Dim Scratch

DataTable (Temp,True,-1)
Sample (1,TCTemp,FP2)
Maximum (1,TCTemp,FP2,False,True)
EndTable

'This table is written to only when trigger occurs
DataTable (TrigTable,Trigger,1000)
Sample (1,TCTemp,FP2)
EndTable

BeginProg
'measurements
Scan (1,Sec,3,0)
PanelTemp (RefTemp,4000)
TCDiff (TCTemp,1,mv34,1,TypeT,RefTemp,True,0,4000,1.0,0)

'Case statements determining whether Trigger occurs
Select Case TCTemp
Case Is >= 80
TriggerSequence(1,0)
Trigger=True
Case Is < 80
TriggerSequence(1,-1)
Trigger=False
EndSelect

CallTable (Temp)
CallTable (TrigTable)
NextScan

SlowSequence
Do
'Proceed in this loop only when trigger occurs
WaitTriggerSequence
SendVariables (SendResult,COMRS232,0,4094,0000,0,"Public","Callback",Scratch,1)
Loop
EndProg
```

## Remarks

The WaitTriggerSequence instruction marks the beginning of code within a SlowSequence scan that is executed when the trigger occurs. WaitTriggerSequence can be placed within a [Do Loop](doloop.md) to have the sequence run continuously (and waiting on the next trigger).

The TriggerSequence instruction triggers the execution of the code. The TimeOut parameter in the TriggerSequence allows the execution of the code to be delayed for a specified amount of time after the trigger condition occurs. Each time TriggerSequence is executed the timeout for that sequence is reset. Thus, the timeout should be no greater than the interval on which the TriggerSequence is executed or the timeout will keep being reset and the code defined by WaitTriggerSequence may not be executed. TriggerSequence can be placed within the main program or within the SlowSequence scan containing the WaitTriggerSequence. Multiple TriggerSequences can be used for one WaitTriggerSequence to set different intervals on which the WaitTriggerSequence code is executed.

This instruction provides a power saving advantage over using a flag to control the execution of a block of code. The datalogger can go into a low power mode when waiting for the sequence to be triggered rather than be constantly powered and monitoring a flag value.

The EndSequence instruction marks the end of a sequence that started with [BeginProg](beginprogendprog.md), a SlowSequence declaration, or a TriggerSequence, and ends any accompanying declaration sequences (such as DataTable declarations). Use of EndSequence prevents insertion of a declaration sequence in the middle of some other executing sequence of code. Declaration sequences must occur before BeginProg, after an EndSequence but before a subsequent SlowSequence, or immediately following a SlowSequence declaration.

## Parameters

# SequenceNum (Slow Sequence Number)

Identifies the SlowSequence in which the code to be executed resides. SlowSequences are numbered in the order they are declared in the program. Up to four slow sequences can be defined in one program.

Type: Constant 1, 2, 3, or 4

# TimeOut (Wait Time)

Indicates the amount of time (0.01 seconds) the datalogger will delay before executing the code marked by the WaitTriggerSequence instruction. If 0 is entered, the code is executed immediately. If a negative number is entered, any pending TriggerSequence for SequenceNum will be canceled.

Type: Constant or Variable
