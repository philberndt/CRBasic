# FieldCal (Calibrate)

The FieldCal instruction sets up the datalogger to perform a calibration of one or more variables in an array.

## Syntax

FieldCal(Function,MeasureVar,Reps,MultVar,OffsetVar,Mode,KnownVar,Index,Avg)

### Example #1

```
'\\\\\\\\\\\\\\\\\\\\ DECLARE VARIABLES //////////////////////////////
Public ZeroMode1,KnownVar1, ZeroMode2,KnownVar2, ZeroMode3
Public AccelA(1), AccelB(1), AccelC(3)
Units ACCELA = GForce : Units AccelB = GForce : Units AccelC = GForce
Public AccelAmult(1), AccelBmult(1),AccelCmult(3), AccelAoset(1), AccelBoset(1), AccelCoset(3)
Alias AccelA(1) = Accel1 : Alias AccelB(1) = Accel2 : Alias AccelC(1) = Accel3
Alias AccelC(2) = Accel4 : Alias AccelC(3) = Accel5
Public RepA, IndexA, RepB, IndexB, RepC, IndexC
Public LoadTest
Dim I

DataTable (ACCEL,True,-1)'Trigger, auto size
DataInterval (0,0,0,0)'Synchronous, 0 lapses, autosize
Sample (1,AccelA(),IEEE4)'1 Reps,Source,Res
Sample (1,AccelB(),IEEE4)'1 Reps,Source,Res
Sample (3,AccelC(),IEEE4)'3 Reps,Source,Res,Enabled
EndTable'End of table ACCEL

DataTable (CalTable,NewFieldCal,100)'Cal Table that stores Calibration values
CardOut (0 ,100)'for retrieval by user for tracking purposes, stores data to a card
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

Scan(200,msec,100,0)
' Input Var,Reps,Range,InChan,Excit mV,Reverse,Delay/Settling,Mult Offset
BrFull(AccelA(),1,mv200C,1,Vx1,1,2500,False,False,100,15000,AccelAmult,AccelAoset())
BrFull(AccelB(),1,mv200C,1,Vx1,1,2500,False,False,100,15000,AccelBmult,AccelBoset())
BrFull(AccelC(),1,mv200C,1,Vx1,1,2500,False,False,100,15000,AccelCmult,AccelCoset())

' Setup a two point Calibration function for AccelA
'(Function,Var,Rep,Multiplier, Offset,Mode,KnownVar,Index,Avg)
FieldCal(2,AccelA(),RepA,AccelAmult(1),AccelAoset(1),ZeroMode1,KnownVar1,IndexA,1)
' Setup an offset function for AccelB
'(Function,Var,Rep,Multiplier, Offset,Mode,KnownVar,Index,Avg)
FieldCal(1,AccelB(),RepB,0,AccelBoset(),ZeroMode2,KnownVar2,IndexB,1)
' Setup a zero function for the 3 reps of AccelC
'(Function,Var,Rep,Multiplier, Offset,Mode, KnownVar,Index, Avg)
FieldCal(0,AccelC(),RepC,0,AccelCoset(),ZeroMode3,0,IndexC,1)
CallTable ACCEL
CallTable CalTable
NextScan
EndProg
```

### Example #2

**NOTE:** This function requires manual interaction as described in the program comments.

The following example uses Fieldcal with * Function*2 (Two Point, Multiplier and Offset). Manual input of a measurement is required. A possible use of this example is calibrating a data logger measurement to a secondary measurement from a hand-held meter.

This program was written for a CR1000X, but other dataloggers can use similar code (voltage ranges, channel numbers, or other parameters may need to be changed to reflect the specifications of the datalogger).

```
SequentialMode

Public Voltages(2)'Array for Logger measurements to be calibrated, MeasureVar
Public CMult(2)={ 1.0, 1.0 }'VoltDiff multiplier value set to 1 initially, MultVar
Public COff(2)'VoltDiff offset value, OffsetVar
Public MeterMeasurment(2)'Variable for manual data input, i.e., KnownVar - handheld voltage meter reading

''''Single value or array for multiple channels
''''Same size as Logger measurement variable
Public FC_Reps=1'Reps
Public FC_iIndex'Variable used to identify channel being calibrated, Index
Public FC_nValsAvg=5'Avg
Public FieldcalMode'Fieldcal Mode or Status, Mode
''''To start calibration:
''''Enter meter reading into RWMeterMeasurment(1) and manually enter 1 into FieldcalMode field
''''When FieldcalMode = 3
''''Enter meter reading into RWMeterMeasurment(2) and manually enter 4 into FieldcalMode field
''''FieldcalMode = 6 when calibration completes or -1, -2, -3, -6 for failure status

Public LoadFieldCalResults As String
Public i As Long
Public PWRON As Boolean = False

''''The NewFieldCal function is a Boolean value that is used as the trigger
''''variable for DataTable so that FieldCal values are stored to a table only when a new calibration
''''has been performed. When a new calibration is performed, NewFieldCal is set to true.
''''Once the NewFieldCal function is tested, it is set back to false.
DataTable(FC_History,NewFieldCal,10)
SampleFieldCal'The SampleFieldCal instruction is used to store the values in the FieldCal file to a data table.
EndTable

BeginProg
LoadFieldCal(False)'Set False - only the fields in the .cal field need to match for the values to load
'LoadFieldCal function returns True if the .cal found was successfully loaded or false if it is not.

'''''Report status of calibration load ''''''''
If LoadFieldCal(False) = true Then
LoadFieldCalResults = "Loadfieldcal succesful"
Else
LoadFieldCalResults = "LoadFieldCal failed"
EndIf
'''' End Report
i=1

Scan(1,Sec,3,0)

'Powering on and off the switched 5V ports on the logger to provide calibration reference voltages to differential ports 2 & 3.
If PWRON = True Then
SWVX (VX1,1 ,1)
SWVX (VX2,1 ,1)
Else
SWVX (VX1,0 ,1)
SWVX (VX2,0 ,1)
EndIf

VoltDiff(Voltages(1),1,mv5000,2,True,500,60,CMult(1),COff(1))
VoltDiff(Voltages(2),1,mv5000,3,True,500,60,CMult(2),COff(2))

FC_iIndex = i
'Options1
' FC_iIndex = i 'Manually increment FC_Index for each measurement variable
' FieldCal(2,Current(),FC_Reps,CMult(),COff(),FieldcalMode,MeterMeasurment(),FC_iIndex,FC_nValsAvg)
'Option2
FC_Reps = 2'FC_Reps = size of Current(), i.e. 2,
'FieldCal increments FC_Index according to the size of the MeasureVar parameter array
'All MeterMeasurements() must be input at same time
FieldCal(2,Voltages(),FC_Reps,CMult(),COff(),FieldcalMode,MeterMeasurment(),FC_iIndex,FC_nValsAvg)
CallTable(FC_History)
NextScan
EndProg
```

## Remarks

When the FieldCal instruction is in a program, a Cal file with calibration values will be written to datalogger memory. The name of the calibration file is the same as the running program that created it with a \*.CAL file extension. The values from this file can be loaded back into the variables in the datalogger if the datalogger experiences a power failure (see [LoadFieldCal (Load Calibration Values from FieldCal)](loadfieldcal.md)). Best practice is to put the FieldCal instruction in the same scan loop as the measurements being calibrated

**NOTE:** It is recommended that the Reps and Index parameters be non-constant variables that are initialized to the desired values after the [BeginProg](beginprogendprog.md) instruction. This rule can be ignored if setting up calibrations on single element variables, and the Mode variable parameter for each FieldCal instruction in the program is represented by a unique variable.

The keyboard display or software interface can be used to set the calibration values in the datalogger.

### Manual Inputs

The following describes how to use FieldCal with * Function*2 (Two Point, Multiplier and Offset) or 3 (Two Point, Multiplier Only) where manual input of a measurement is required. An example is calibrating a datalogger measurement to a secondary measurement from a hand-held meter.

The transition through the various calibration steps or * Modes*is done automatically except * Modes*1 and 4. Once the 1st calibration data point is in the variable _ MeasureVar_, the FieldCal * Mode*is manually changed from 0 to 1 to start the process. The FieldCal function reads the datalogger input being calibrated multiple times as indicated by the * Avg*parameter. FieldCal will automatically increment from * Mode*1 to 2 and from 2 to 3.* Mode *3 indicates the data has been processed and FieldCal is ready for the 2nd set of data points. Once the 2nd data point is input into the* MeasureVar *variable the FieldCal* Mode *is manually changed to 4 to initiate the processing of the 2nd set of data points and perform the calibration calculations. The FieldCal function reads the datalogger input being calibrated multiple times as indicated by the* Avg *parameter. FieldCal will automatically increment from* Mode *4 to 5 and from 5 to 6.* Mode *6 indicates the calibration is complete.

Here is the process used to perform Two Point, Multiplier and Offset calibration with manual manipulation of FieldCal Mode and manual data entry:

1. Set the system being measured to the 1st of two measurements (ex. lowest voltage setting). The datalogger reads the 1st data point and puts the value into the* MeasureVar(1)*variable.

2. Enter the 1st value (from the meter reading) into the * KnownVar*variable

3. The * Mode*starts at 0 by default. Manually or programmatically set the * Mode*to 1 to initiate the calibration routine.

4. The Fieldcal * Mode*will change to 2 indicating that it is calculating and then change to 3 when the 1st data point processing is complete

5. Set the system being measured to the 2nd of two measurements (ex. highest voltage setting). The datalogger reads the 2nd data point and puts the value into the * MeasureVar(1)*variable.

6. Enter the 2nd value (from the meter reading) into the * KnownVar*variable

7. Manually or programmatically set the * Mode*to 4 to initiate the calibration routine.

8. The Fieldcal * Mode*will change to 5 indicating that it is calculating and then change to 6 when the processing is complete

9. The * MultVar*and * OffsetVar*variables are populated with the multiplier and offset values. Use these values in commands like VoltDiff for calibrated measurements.

See Example #2 for a program that uses this process.

See the Calibration and Zeroing section in the LoggerNet or RTDAQ manual for more information on using the FieldCal instruction.

## Parameters

# Function

Specifies the type of calibration to be performed. Right click the parameter to display a list:

| Code | Calibration Type                                                                                                                                                                                                                                                               |
| ---- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| 0    | Zeroing Calibration. The offset value is adjusted so that the measurement result is exactly zero while the sensor is in the calibrated state (known zero condition). The multiplier is not adjusted.                                                                           |
| 1    | Offset Calibration. The offset value is adjusted so that the measurement result gives a specified, desired value while the sensor is in its calibrated state (known condition). The multiplier is not adjusted.                                                                |
| 2    | Two Point, Multiplier and Offset. The multiplier and offset values are adjusted to make a linear fit between the two calibration input states (two known conditions).                                                                                                          |
| 3    | Two Point, Multiplier Only. The multiplier value is adjusted to make a best fit between the two calibration input states (two known conditions). The offset is not adjusted.                                                                                                   |
| 4    | Zero Basis Point. A measurement value (usually non-zero) is stored for later reference, comparison, or calculation purposes (Baseline or Datum point). This value corresponds to a zero condition or baseline sensor state. Neither the multiplier nor the offset is adjusted. |

Type: Integer

# MeasureVar (Variable or Variable Array to Calibrate)

The variable or variable array being calibrated.

Type: Variable

Note that for the FieldCal instruction, the MeasureVar parameter must be dimensioned large enough to accommodate the number of Reps.

# Reps (Repetitions)

The Reps parameter is a constant or variable used to specify how many values in the array to calibrate. Reps must be equal to either 1 or the size of the MeasureVar parameter array. When Reps is equal to the size of the MeasureVar array, all elements of the MeasureVar array will be calibrated in a single scan (the Index parameter must be set to 1). When Reps is set to 1, a single element of the MeasureVar array, specified by the Index parameter will be calibrated.

If the Reps parameter is declared as a variable, the number of reps can be changed during program operation. This allows the calibration of a complete array or a single element at different points in time. Reps should be initialized to either 1 or the size of the MeasureVar array prior to starting a calibration. If Reps is set to zero, no calibration will occur for this instruction.

Type: Constant or Variable

# MultVar (Multiplier Variable)

The MultVar parameter is the variable or variable array that will be populated with the computed Multiplier(s) from the calibration(s). This variable should also be used for the Multiplier parameter in the measurement instruction for which the calibration is being made. MultVar should be dimensioned to the same size as the MeasureVar variable array. The element of the array for the calibration is set by the Index parameter. If MultVar is equal to 0 or NAN at the start of calibration, it will be set to 1 during the calibration process.

Type: Variable

# OffsetVar (Offset Variable)

The OffsetVar parameter is the variable or variable array that will be populated with the computed Offset(s) from the calibration(s). This variable should also be used for the Offset parameter in the measurement instruction for which the calibration is being made. When performing a Multiplier Only Two Point calibration, zero can be entered for this parameter. When performing any other calibration function, the OffsetVar should be dimensioned to the same size as the MeasureVar variable array. The element of the array for the calibration is set by the Index parameter. If OffsetVar is equal to NAN at the start of calibration, it will be set to 0 during the calibration process.

Type: Variable

# Mode (Current State)

A variable that stores an integer that indicates the current state of the calibration. This is both a trigger value to start a process and a status value reporting an operational state or error state. States are as follows:

| Value Returned | State                                                                                                                                                                                                                         |
| -------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| -1             | Error in the calibration setup                                                                                                                                                                                                |
| -2             | Multiplier set to 0 or =NAN **Note:** Not a number. A data word indicating a measurement or processing error. Voltage overrange, SDI-12 sensor error, and undefined mathematical results can produce NAN. ; measurement = NAN |
| -3             | Reps is set to a value other than 1 or the size of MeasureVar                                                                                                                                                                 |
| 0              | No calibration                                                                                                                                                                                                                |
| 1              | Ready to calculate (KnownVar holds the first of a two point calibration)                                                                                                                                                      |
| 2              | Working                                                                                                                                                                                                                       |
| 3              | First point done (only applicable for two point calibration)                                                                                                                                                                  |
| 4              | Ready to calculate (KnownVar holds the second of a two point calibration)                                                                                                                                                     |
| 5              | Working (only applicable for two point calibration)                                                                                                                                                                           |
| 6              | Calibration complete                                                                                                                                                                                                          |
| -6             | New calibration attempted before datalogger is ready for calibration (one full scan must execute after calibration is complete, and before another calibration can be started)                                                |

If multiple FieldCal instructions are used in the program, you must define a Mode variable for each instruction.

Type: Variable

# KnownVar (Known Variable)

The KnownVar parameter is a variable array that holds the set point value(s) to be used in the calibration routine. 0 can be entered when performing a Zero calibration. When performing any other calibration function, the KnownVar must be dimensioned to the same size as the MeasureVar. The element of the array used for the first calibration is set by the Index parameter.

Type: Variable

# Index

If Reps is set to 1, then Index specifies which element of the MeasureVar array will be calibrated. If Reps is set to the size of the MeasureVar, then Index must be set equal to 1 (complete array will be calibrated starting with the first element). This parameter can be a constant or a variable. It must be initialized to a non-zero value before a calibration can be performed.

Type: Constant or Variable

# Avg (Average)

Specifies the number of points to average when performing the calibration.

Type: Variable or Integer

In the Calibration Wizard (available from RTDAQ or the LoggerNet Connect screen), if a program has been configured to zero multiple measurements using the same mode variable, and the Reps argument of the FieldCal instruction is using a variable, then specific measurements can be excluded from the calibration by setting the value of the Reps variable to zero. This will ensure that the measurement is not calibrated, even though its mode variable is activated.

**NOTE:** When performing the calibration, the FieldCal instruction automatically compensates for any existing multiplier and offset for the measurement instruction. There is no need for the user to do this under program control.
