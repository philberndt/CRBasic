# LoadFieldCal (Load Calibration Values from FieldCal)

The LoadFieldCal function is used to load values from the FieldCal file into variables in the datalogger.

## Syntax

LoadFieldCal(CheckSig)

```
'\\\\\\\\\\\\\\\\\\\\ DECLARE VARIABLES //////////////////////////////
Public ZeroMode1,KnownVar1, ZeroMode2,KnownVar2, ZeroMode3,KnownVar3(3)
Public AccelA(1), AccelB(1), AccelC(3)
Units ACCELA = GForce : Units AccelB = GForce : Units AccelC = GForce
Public AccelAmult(1), AccelBmult(1),AccelCmult(3), AccelAoset(1), AccelBoset(1), AccelCoset(3)
Alias AccelA(1) = Accel1 : Alias AccelB(1) = Accel2 : Alias AccelC(1) = Accel3
Alias AccelC(2) = Accel4 : Alias AccelC(3) = Accel5
Public RepA, IndexA, RepB, IndexB, RepC, IndexC
Public LoadTest, Flag(8)
Dim I

DataTable (ACCEL,True,-1)'Trigger, auto size
DataInterval (0,0,0,0)'Synchronous, 0 lapses, autosize
Sample (1,AccelA(),IEEE4)'1 Reps,Source,Res
Sample (1,AccelB(),IEEE4)'1 Reps,Source,Res
Sample (3,AccelC(),IEEE4)'3 Reps,Source,Res,Enabled
EndTable'End of table ACCEL

DataTable (CalTable,NewFieldCal,100)'Cal Table that stores Calibration values

SampleFieldCal
EndTable

BeginProg'Program begins here
'MainSequence
'Initialize rep and Index parameters
RepA = 1 : IndexA = 1 : RepB = 1 : IndexB = 1 : RepC = 3 : IndexC = 1
KnownVar2 = 1' Set the offset value for ACCELB
AccelAMult(1) = 1 : AccelBMult(1) = 1: AccelAoset(1) = 0 : AccelBoset(1) = 0
For I = 1 To 3'Do the following to all of BBlk2
AccelCmult(I)=1 : AccelCoset(I) = 0'Initialize mult & offset values for ACCELC
Next I'Repeat above until finished
LoadTest =LoadFieldCal(0)

Scan(200,mSec,100,0)'Scan once every 100 mSec, 100 Scan Buffer, non-burst
' Input Var,Reps,Range,InChan,Excit mV,Reverse,Delay/Settling,Mult Offset
BrFull(AccelA(),1,mv34,1,Vx1,1,2500,False,False,100,4000,AccelAmult,AccelAoset())
BrFull(AccelB(),1,mv34,2,Vx1,1,2500,False,False,100,4000,AccelBmult,AccelBoset())
BrFull(AccelC(),1,mv34,3,Vx1,1,2500,False,False,100,4000,AccelCmult,AccelCoset())

'Setup a two point Calibration function for AccelA
'(Function,Var,Rep,Multiplier, Offset,Mode,KnownVar,Index,Avg)
FieldCal(2,AccelA(),RepA,AccelAmult(1),AccelAoset(1),ZeroMode1,KnownVar1,IndexA,1)
'Setup an offset function for AccelB
'(Function,Var,Rep,Multiplier, Offset,Mode,KnownVar,Index,Avg)
FieldCal(1,AccelB(),RepB,0,AccelBoset(),ZeroMode2,KnownVar2,IndexB,1)
'Setup a zero function for the 3 reps of AccelC
'(Function,Var,Rep,Multiplier, Offset,Mode, KnownVar,Index, Avg)

FieldCal(0,AccelC(),RepC,0,AccelCoset(),ZeroMode3,0,IndexC,1)
CallTable ACCEL
CallTable CalTable
NextScan
EndProg
```

## Remarks

Before loading the FieldCal values, the datalogger will check the program signature of the currently running program and the program signature stored in the FieldCal file. The CheckSig parameter is aBoolean

**Note:** Data type used to represent conditions or hardware that have only two states (true or false) such as flags and control ports.

variable that determines whether or not the datalogger will load the FieldCal values if the program signatures do not match. If CheckSig is True, the program signature must match the signature in the .cal file for the values to load. If CheckSig is False, only the fields in the .cal field need to match for the values to load. The LoadFieldCal function returns True if the .cal found was successfully loaded or false if it is not.

If there is a fairly insignificant program change made that does not affect the calibrated values in the program, it may be desirable to load the FieldCal values even though the program signature has changed. Setting CheckSig to False will allow this. However, if the datalogger detects any changes in the structure of the FieldCal file, the LoadFieldCal function will fail. LoadFieldCal can be used to monitor whether or not the loading of values is successful.
