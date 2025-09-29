# NewFieldCal (Store FieldCal Values)

The NewFieldCal function is used in a data table declaration to trigger storage of [FieldCal](fieldcal.md) values when a new FieldCal file has been written.

## Syntax

DataTable ( TableName,NewFieldCal, Size)

SampleFieldCal

EndTable

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
CardOut (0 ,100)'for retrieval by user for tracking purposes
SampleFieldCal
EndTable

BeginProg'Program begins here
'MainSequence
'Inilialize rep and Index parameters
RepA = 1 : IndexA = 1 : RepB = 1 : IndexB = 1 : RepC = 3 : IndexC = 1
KnownVar2 = 1' Set the offset value for ACCELB
AccelAMult(1) = 1 : AccelBMult(1) = 1: AccelAoset(1) = 0 : AccelBoset(1) = 0
For I = 1 To 3'Do the following to all of BBlk2
AccelCmult(I)=1 : AccelCoset(I) = 0'Initialize mult & offset values for ACCELC
Next I'Repeat above until finished
LoadTest = LoadFieldCal(0)

Scan(200,mSec,100,0)'Scan once every 100 mSec, 100 Scan Buffer, non-burst
' Input Var,Reps,Range,InChan,Excit mV,Reverse,Delay/Settling,Mult Offset
BrFull(AccelA(),1,mv200C,1,Vx1,1,2500,False,False,100,15000,AccelAmult,AccelAoset())
BrFull(AccelB(),1,mv200C,2,Vx1,1,2500,False,False,100,15000,AccelBmult,AccelBoset())
BrFull(AccelC(),1,mv200C,3,Vx1,1,2500,False,False,100,15000,AccelCmult,AccelCoset())

' Setup a two point Calibration function for AccelA
'(Function,Var,Rep,Multiplier, Offset,Mode,KnownVar,Index,Avg)
FieldCal(2,AccelA(),RepA,AccelAmult(1),AccelAoset(1),ZeroMode1,KnownVar1,IndexA,1)
' Setup an offset function for AccelB
'(Function,Var,Rep,Multiplier, Offset,Mode,KnownVar,Index,Avg)
FieldCal (1,AccelB(),RepB,0,AccelBoset(),ZeroMode2,KnownVar2,IndexB,1)
' Setup a zero function for the 3 reps of AccelC
'(Function,Var,Rep,Multiplier, Offset,Mode, KnownVar,Index, Avg)

FieldCal (0,AccelC(),RepC,0,AccelCoset(),ZeroMode3,0,IndexC,1)
CallTable ACCEL
CallTable CalTable
NextScan
EndProg
```

## Remarks

The NewFieldCal function is aBoolean

**Note:** Data type used to represent conditions or hardware that have only two states (true or false) such as flags and control ports.

value that is used as the trigger variable for DataTable so that FieldCal values are stored to a table only when a new calibration has been performed. When a new calibration is performed NewFieldCal is set to true. Once the NewFieldCal function is tested, it is set back to false.
