# SDMBeginPort (Specify SDM Control Ports)

The SDMBeginPort instruction is used to specify which ports to use for controlling an SDM.

## Syntax

SDMBeginPort(SDMPort)

In the following example, a counter is used to fill an array called Src( ) that will control two SDM-CD16ACs. The SDMBeginPort instruction is used to configure the start port toC1.

```
'Declare Variables
Public src(32)
Dim i, count, mask(16)
SDMBeginPort(C1)
'Program
BeginProg
For i=1 To 16
mask(i) = 2^(i-1)
Next i
Scan(20,msec,2,0)
count = count + 1
For i=1 To 32
src(i) = count AND mask(((i-1) MOD 16) +1)
Next i
SDMCD16AC (src(),2,1)
NextScan
EndProg
```

## Remarks

This instruction is used to specify the first of three consecutive ports that will be used for controlling an SDM device. If this instruction is omitted from the program, SDM control occurs on C1, C2, and C3 and the device should be wired accordingly. SDMBeginPort must be placed in the program before the BeginProg instruction and before any SerialOpen that will access the port set up for SDM. Only one SDMBeginPort instruction is allowed in a program.

## Parameter

# SDMPort

Used to specify the C ports to use for controlling an SDM device. Enter the number of the first port in a series of three:

C1 C1, C2, C3

C5 C5, C6, C7
