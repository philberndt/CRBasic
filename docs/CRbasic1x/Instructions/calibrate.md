# Calibrate

The Calibrate instruction is used to force calibration of all analog channels under program control. Calibration is typically performed to compensate for errors in voltage measurements due to temperature.

## Syntax

Calibrate(Dest,Range) (parameters are optional)

The example shows how to use the Calibrate instruction in a SlowSequence to calibrate the datalogger every sixty seconds.

```
'Calibrate
Public Temp1
Public Calib1(60)
DataTable(Table1,1,600)
DataInterval(0,1,sec,1)
Sample(1,Temp1,FP2)
EndTable

BeginProg
Scan(20,mSec,0,0)
PanelTemp (Temp1,15000)
CallTable Table1
NextScan

SlowSequence
Scan (60,Sec,0,0)
Calibrate(Calib1)
NextScan
EndProg
```

## Remarks

During calibration, the datalogger measures offset and gain on voltage ranges and calculates calibration coefficients. Calibration occurs when a datalogger program is compiled (typically, when the datalogger is powered up or when a watchdog error occurs). Calibration also occurs automatically every 36 to 364 seconds during program execution. The frequency of calibration is based on the lowest notch frequency (fN1) used in the program; maximum is 1000 milliseconds. When the Calibrate instruction is used in a program, this automatic calibration is disabled and calibration occurs when the instruction is executed. When CDMs are used with the datalogger, this instruction will cause the datalogger and all modules to perform their respective calibration measurements.

Temperature is the major factor affecting the calibration of the analog voltage measurement section. If calibration is not performed as part of the program, a typical shift in the calibration is 0.01% per degree C change from the temperature at which the program compile calibration occurred. When there is adequate time for all measurements, calibration is performed in the background to provide continuous adjustment as temperature changes. If there is not enough time to perform automatic calibration, you may want to enable it under program control if accuracy is important. If the program does not allow enough time for automatic calibration to be performed, the message "Warning when Fast Scan x is running background calibration will be disabled" will be returned when the program is compiled.

The instruction can be placed in a fast scan with measurements, or, if there is insufficient time to run it in the fast scan it can be placed in a [SlowSequence](slowsequence.md). When the instruction is placed in a SlowSequence, calibration is performed as extra time slices allow. Note, however, that unless the SlowSequence is faster than about 40 seconds, the calibration will not be performed any faster than if using automatic calibration.

Parameters for this instruction are optional, and are used only when the user desires to store the results of the calibration. The Dest parameter should reference an array dimensioned to at least 60 values to store the results. If your program also includes [BrFull](brfull.md),[BrHalf3W](brhalf3w.md), or [BrHalf](brhalf.md) you will need to increase the array size by one value for each excitation channel used in these instructions. The Range parameter is a constant entered as either 0 or non-zero. It allows the user to choose whether to perform a calibration on all ranges or only those ranges used in the program (Range = 0, calibrate only ranges used in program; Range = non-zero, calibrate all ranges).

**Caution:** Do NOT place this instruction inside a conditional statement when running inpipeline mode

**Note:** A CRBasic program execution mode wherein instructions are evaluated in groups of like instructions, with a set group prioritization.

.

## Optional Parameters

# Dest

The array in which to store the calibration coefficients. The array must be dimensioned to 60 in order to hold all of the calculated calibration coefficients.

Type: Array

# Range

The entry for this optional parameter is either 0 or a non-zero number.

| Code | Description                                      |
| ---- | ------------------------------------------------ |
| 0    | Calibrate only those ranges used in the program. |
| ≠ 0  | Calibrate all ranges.                            |

Type: Constant or a Variable
