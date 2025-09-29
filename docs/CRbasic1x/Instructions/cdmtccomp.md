# CDM_TCComp (CDM Thermocouple Measurement with Cold Junction Compensation)

The CDM_TCComp instruction is used to perform differential thermocouple measurements with automatic cold-junction compensation using a TEMP 120, VOLT 116, VOLT 108, or a CDM-A100 with CDM-A100 OS version 4 or greater. The CDM_TCComp instruction automatically employs input reversal to enhance measurement quality and open detection to help detect sensor wiring issues

## Syntax

CDM_TCComp(CDMType,CPIAddress,Dest,Reps,DiffChan,TCType,FilterEnable,Units)

The following program demonstrates using the CDM_TCComp instruction to measure 120 Type T thermocouples using 6 CDM-Temp 120 modules.

```
Public TC(120) : Units TC = C
Alias TC(1) = ExhaustGasTemp

Const TC_TYPE = TYPET

DataTable (TableTemps,True,-1)
DataInterval (0,1,SEC,10)
Average (120,TC(),IEEE4,False)
Maximum (120,TC(),IEEE4,False,True)
Minimum (120,TC(),IEEE4,False,True)
EndTable

BeginProg
Scan (1,Sec,100,0)
'FiltEn - 0=Disabled
' 1 =Enabled (simultaneous 50/60Hz filtering)
'Units - 0=Celsius
' 1=Fahrenheit
' 2=Kelvin

'CDM_TCComp(DevType, CPIAddr, Dest, Reps, TCCh, TCType, FiltEn, Units)
CDM_TCComp(TEMP120, 1, TC( 1), 20, 1, TC_TYPE, 1, 0)
CDM_TCComp(TEMP120, 2, TC( 21), 20, 1, TC_TYPE, 1, 0)
CDM_TCComp(TEMP120, 3, TC( 41), 20, 1, TC_TYPE, 1, 0)
CDM_TCComp(TEMP120, 4, TC( 61), 20, 1, TC_TYPE, 1, 0)
CDM_TCComp(TEMP120, 5, TC( 81), 20, 1, TC_TYPE, 1, 0)
CDM_TCComp(TEMP120, 6, TC(101), 20, 1, TC_TYPE, 1, 0)
CallTable(TableTemps)
NextScan
EndProg
```

## Remarks

In contrast to the [CDM_TCDiff](cdmtcdiff.md) instruction, the CDM_TCComp instruction automatically performs cold-junction compensation using data from the junction temperature sensor nearest the specified TC input(s). I.e., there is no reference temperature parameter for this instruction. In addition, the CDM_TCComp instruction automatically employs input reversal to enhance measurement quality and open detection to help detect sensor wiring issues.

The calculations used for the CDM_TCComp instruction are based on the thermocouple type selected (TCType). The instruction adds the measured voltage to the voltage calculated for the reference temperature relative to 0 degrees Celsius and converts the combined voltage to temperature in user-selected units (refer to the Units parameter). Multiple CDMs may be used to increase the number of thermocouple measurements. Each CDM module on the bus will require its own CDM_TCComp instruction.

**NOTE:** This instruction must NOT be placed inside a conditional statement when running in pipeline mode.

## Parameters

# CDMType (CDM Type)

Used to specify the type ofCDM **Note:** used by the instruction. This is a read-only setting in the device which cannot be changed. Right-click the parameter to display a list of valid CDM types.

Type: Constant

# CPIAddress (CPI Address)

TheCPI

** Note:**CPI is a proprietary interface for communications between Campbell Scientific dataloggers and Campbell Scientific CDM peripheral devices. It consists of a physical layer definition and a data protocol. CDM devices are similar to Campbell Scientific SDM devices in concept, but the use of the CPI bus enables higher data-throughput rates and use of longer cables. CDM devices require more power to operate in general than do SDM devices.

address configured in theCDM ** Note:** module. Valid range is 1 through 120. Note that the CPIAddress cannot be a variable because the datalogger must know all information for a given CDM module at program compile.(e.g., before the program runs).

Type: Constant integer

# Dest (Destination)

The Variable in which to store the results of the instruction. Right-click the parameter to display a list of defined variables.

If this instruction has a Repetitions parameter and it is greater than 1, the results are stored in an array with the variable name. The array must be dimensioned large enough to hold all of the values returned from all of the Reps.

Type: Variable or Array

# Reps (Repetitions)

The number of repetitions for the measurement or instruction.

Type: Constant integer (or expression that evaluates as a constant).

# DiffChan (Differential Channel)

The number of the differential channel pair on which to make the first measurement. The number of available channels depends upon the CDM being programmed. If the Reps parameter is greater than 1, the additional measurements will be made on sequential channels.

If the DiffChan number is entered as a negative value, all Reps will be performed on the same channel (burst measurement). The burst mode sample frequency is controlled by the fN1 parameter, so there are 16 sample frequencies with a maximum of 30 kHz. The SettlingTime parameter is used to delay once prior to beginning the burst. The total time prior to beginning the burst is the SettlingTime plus 180 microseconds.

Type: Constant

# TCType (Thermocouple Type)

The TCType argument is used to identify the type of thermocouple being measured. An alphanumeric code is entered. Right-click on the parameter to display a list of valid options.

| Alphanumeric | Type               | (+Lead/-Lead)       |
| ------------ | ------------------ | ------------------- |
| TypeT        | Copper Contstantan | Cu/Cu-Ni            |
| TypeE        | Chromel Constantan | Ni-Cr/Cu-Ni         |
| TypeK        | Chromel Alumel     | Ni-Cr/Ni-Al         |
| TypeJ        | Iron Constantan    | Fe/Cu-Ni            |
| TypeB        | Platinum Rhodium   | Pt-30% Rh/ Pt-6% Rh |
| TypeR        | Platinum Rhodium   | Pt-13% Rh/Pt        |
| TypeS        | Platinum Rhodium   | Pt-10% Rh/Pt        |
| TypeN        | Nicrosil-Nisil     | Ni-Cr-Si/Ni-Si-Mg   |

** NOTE:**When using TEMP408 modules, the TCType selected must match the module type.

Type: Constant

# FilterEnable

If the FilterEnable parameter is set to 1 (enabled), 50 Hz and 60 Hz frequencies will be eliminated or notched out simultaneously by the sync filter. Note that when the filter is enabled, the fastest that measurements can be made is 1 Hz for all 20 channels. In contrast, if the FilterEnable parameter is set to 0 (disabled), the TEMP120 can measure all 20 channels at speeds up to 10 Hz.

| Option | Description                           |
| ------ | ------------------------------------- |
| 0      | Filter disabled for fast measurements |
| 1      | Filter 50/60 Hz noise (AC Power)      |

Type: Constant

# Units

Used to specify the units of the returned measurement.

Valid options for the units parameter are:

| Numeric | Unit       |
| ------- | ---------- |
| 0       | Celsius    |
| 1       | Fahrenheit |
| 2       | Kelvin     |

Type: Constant
