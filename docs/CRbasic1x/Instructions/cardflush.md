# CardFlush (Write Buffered Data to an External Device)

The CardFlush instruction is used to immediately write any buffered data from the datalogger's internal memory and file system to an external device.

## Syntax

CardFlush

In the following example program, all data buffered in the CPU will be written to the PC card when Flag(1) is set high by the user.

```
Public VoltMsmt(5), Flag(4)

DataTable (Measure,True,1000)
CardOut (0 ,-1)
Sample (5,VoltMsmt,FP2)
EndTable

BeginProg
Scan (1,Sec,3,0)
VoltDiff (VoltMsmt,5,mV5000,1,False,0,250,1.0,0)

If Flag(1) ThenCardFlush
CallTable (Measure)
NextScan
EndProg
```

## Remarks

This instruction is used to immediately write any buffered data to a card when using a CardOut instruction. It can also be used to write any data buffered from a TableFile instruction to a CRD or USB(SC115)drive.

This instruction does not replace pressing the Card Control button prior to removing the card. If CardFlush is executed and the card removed without pressing the Control button, the data will be available on the card for conversion but the same card cannot be reinserted unless all the files are deleted.
