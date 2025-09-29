# FieldCalStrain (Calibrate Strain)

The FieldCalStrain instruction sets up the datalogger to perform a zero or shunt calibration for a strain measurement.

## Syntax

FieldCalStrain(Function,MeasureVar,Reps,GFAdj,ZeromV/V,Mode,KnownRS,Index,Avg,GFRaw,uStrainDest)

```
'\\\\\\\\\\\\\\\\\\\\ DECLARE VARIABLES /////////////////////////////
Const Reps = 2'Set program to measure 2 strain gauges
Const BrConfig = -4'Block1 gauge code for Full bridge strain, Bending
Dim I'Declare I as a variable
Public NumAvg, CalFileLoaded, Flag(8)
'Variables that are arguments in the Zero Function
Public ModeZero, ZeroReps, index0
Public RawmVperV(Reps)
Public ZeroMvperV(Reps)
'Variables that are arguments in the Shunt Function
Public ModeShunt, KnownRes(Reps), IndexS
Public MeasureVar_uS(Reps)
Public GF_Adj(Reps), GF_Raw(Reps)
Public ZeroStrain(Reps)

'---------------------------- Tables----------------------------
DataTable(Table1,True,-1)'Trigger, auto size
DataInterval(0,1000,mSec,100)
Average(Reps,MeasureVar_uS(),IEEE4,False)
EndTable

DataTable(CalHist,NewFieldCal,50)
SampleFieldCal
EndTable

'\\\\\\\\\\\\\\\\\\\\\\\\\\\\ PROGRAM ////////////////////////////
BeginProg
NumAvg = 10'Initialize the number of values to average for the calibrations
IndexS = 1'Initialize shunt Index to 1
Index0 = 1'Initialize zero index to 1
Zeroreps = Reps'Initialize ZeroReps to full size of array
'Set Gauge Factors
GF_Raw(1) = 2.1 : GF_Raw(2) = 2.1 : GF_Raw(3) = 2.13
For I = 1 To Reps'Initialize the Adj Gauge Factors to the raw GF value
GF_Adj(I) = GF_Raw(I)'The adj Gauge factors are used in the calculation of uStrain
Next I
'If a calibration has been done, the following will load the zero or Adjusted GFfrom the Calibration file
CalFileLoaded = LoadFieldCal(1)
Scan(200,mSec,100,0)
BrFull (RawmVperV(),Reps,mv34,1,Vx1,1,2500,True ,True,0,4000,1.0,0)

StrainCalc(MeasureVar_uS(),Reps,RawmvperV(),ZeroMvperV(),BrConfig,GF_Adj(),0)'Strain calculation
If Flag(8) then
ZeroReps = Reps'Set Reps to zero complete measurementarray
Index0 = 1'Verify that the index is at the beginning of the array
ModeZero = 1'Set the Mode for the zero function to 1 to start the zero process
Flag(8) = 0'Set the zero flag back to low
Endif
'FieldCalStrain(Zeroing,Mvar,reps,GF_adj,Zeromv_V,ModeVar,KnownVar,index,Numavg,GF_Raw)
FieldCalStrain(10,RawmvperV(),ZeroReps,0,ZeroMvperV(),ModeZero,0,index0,NumAvg,0, ZeroStrain())
'FieldCalStrain(Shunt,Mvar,reps,GF,Zerooffset,ModeVar,KnownVar,index,Numavg,GF_Raw)
FieldCalStrain(43,MeasureVar_uS(),1,GF_Adj(),0,ModeShunt,KnownRes,IndexS,NumAvg,GF_Raw(),0)
CallTable Table1
CallTable CalHist
Next Scan
EndProg
```

## Remarks

This instruction is a specialized form of the FieldCal instruction used to perform zeroing and shunt calibrations on quarter bridge strain, half bridge bending strain, and full bridge bending strain measurements using the StrainCalc function. It should be noted that Shunt Calibration does not calibrate the strain gauge, but adjusts the gauge manufacturer supplied calibration gauge factor (GF) to compensate for errors introduced by non-linearity in the Wheat-stone bridge, long leads, and/or errors in the measurement system.

When the FieldCalStrain instruction is in a program, a Cal file with calibration values is written to datalogger memory. The location (CPU or Card) of this file is the same as the running program that created it. The name of the calibration file is the same as the running program that created it with a \*.CAL file extension. The values from this file can be loaded back in to the variables in the datalogger if the datalogger experiences a power failure (see [LoadFieldCal](loadfieldcal.md)).

**NOTE:** It is recommended that the Reps and Index parameters be declared as non-constant variables that are initialized to the desired values after the [BeginProg](beginprogendprog.md) instruction. This rule can be ignored if setting up calibrations on single element variables, and the Mode variable parameter for each FieldCalStrain instruction in the program is represented by a unique variable.

The keyboard display or software interface can be used to set the calibration values in the datalogger.

## Parameters

# Function (Calibration Type)

Specifies the type of calibration to be performed. Right click the parameter to display a list:

| Code | Description                                  |
| ---- | -------------------------------------------- |
| 10   | Zeroing calibration                          |
| 13   | Quarter bridge strain shunt calibration      |
| 33   | Bending half bridge strain shunt calibration |
| 43   | Bending full bridge strain shunt calibration |

Type: Integer

# MeasureVar (Variable or Variable Array to Calibrate)

The variable or variable array holding the result of the StrainCalc instruction for the gauges being calibrated. The MeasureVar must be dimensioned large enough to accommodate the number of Reps.

Type: Variable

# Reps (Repetitions)

Specifies how many values to calibrate. Reps must be set to 1 or the number of elements in the MeasureVar array. When Reps is equal to the size of the MeasureVar parameter, all elements of the MeasureVar array will be calibrated in a single scan. When Reps is set to 1, the element of the MeasureVar specified by the Index parameter will be calibrated.

Type: Constant or Variable

# GFAdj (Gauge Factor Adjustment)

Not used for zero calibration. Enter 0.

For shunt calibration, this parameter is the variable or variable array that is populated with the computed gauge factors used in the StrainCalc instruction for computing the microstrain. It should be dimensioned large enough to hold values for all the elements in the MeasureVar array. GFAdj is set equal to GFRaw during the calibration process.

Type: Variable or variable array

# ZeromV/V (Zero mV/V)

For Zero calibration, this parameter is the variable or variable array that will be populated with the zero mV/V values. It should be dimensioned large enough to hold values for all of the Reps of the source MeasureVar array. If ZeromV/V is a NAN, it will be set to 0 during the calibration process.

ZeromV/V is not used for shunt calibration. Enter 0

Type: Variable or variable array

# Mode (Current State)

A variable that stores an integer that indicates the current state of the calibration. States are as follows:

| Value Returned | State                                                                                                                                                                                                                         |
| -------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| -1             | Error in the calibration setup                                                                                                                                                                                                |
| -2             | Multiplier set to 0 or =NAN **Note:** Not a number. A data word indicating a measurement or processing error. Voltage overrange, SDI-12 sensor error, and undefined mathematical results can produce NAN. ; measurement = NAN |
| -3             | Reps is set to a value other than 1 or the size of MeasureVar                                                                                                                                                                 |
| 0              | No calibration                                                                                                                                                                                                                |
| 1              | Ready to calculate                                                                                                                                                                                                            |
| 2              | Working                                                                                                                                                                                                                       |
| 3              | Ready to enter the shunt resistance into KnownRs (only for shunt calibration)                                                                                                                                                 |
| 4              | Ready to calculate                                                                                                                                                                                                            |
| 5              | Working (only applicable for two point calibration)                                                                                                                                                                           |
| 6              | Calibration complete                                                                                                                                                                                                          |

Type: Variable

# KnownRs (Known Resistance)

The KnownRs parameter is a variable array that holds the set point value(s) to be used in the calibration routine. 0 can be entered when performing a Zero calibration. When performing any other calibration function, the KnownVar must be dimensioned to the size of MeasureVar. If Reps is set to 1, the element of the array used for the calibration is set by the Index parameter.

Enter the resistance value as a positive number if shunting across the arm that holds the strain gauge for function 13, the arm that holds the gauge that is parallel to +ε for function 33, or the arm that holds the gauge that is parallel to to +ε for function 43. Enter the resistance value as a negative number if shunting across the arm that holds the completion resistor for function 13, the arm that holds the gauge that is parallel to -ε for function 33, or the arm that holds the gauge that is parallel to -ε for function 43.

Type: Variable

# Index

If Reps is set to 1, then Index specifies which element of the MeasureVar array will be calibrated. If Reps is set to the size of the MeasureVar, then Index must be set equal to 1 (complete array will be calibrated starting with the first element). This parameter can be a constant or a variable. It must be initialized to a non-zero value before a calibration can be performed.

Type: Constant or Variable

# Avg (Average)

Specifies the number of points to average when performing the calibration.

Type: Variable or Integer

# GFRaw (Gauge Factor)

Zero calibration: Zero can be entered for this parameter (not used).

Shunt calibration: When setting up a Shunt Calibration, this is the variable or variable array that holds the raw gauge factor(s) for the strain gauges. It should be a different array than that used for the adjusted gauge factors in the StrainCalc instruction, and the value(s) should never be changed. This variable array must be dimension to the same size as the MeasureVar.

Type: Variable array

# uStrainDest (Micro-Strain Result)

Zero calibration: Variable array that is used to store the micro-strain reading result from the StrainCalc instruction. Informs the Zero wizard of the variable array that is being zeroed. Should be dimensioned to the size of the MeasVar.

Shunt calibration: This value is not required. Enter 0.

Type: Variable or 0
