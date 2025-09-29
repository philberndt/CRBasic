# CDM_PanelTemp (CDM Panel Temperature)

The CDM_PanelTemp instruction is used to read the temperature of theCDM **Note:** wiring panel and output the value in degrees Celsius. For additional information on controller area network peripherals and Campbell modules, see [CPI and CDM](https://www.campbellsci.com/news-cpi-cdm).

## Syntax

```
CDM_PanelTemp
```

(CDMType,CPIAddress,Dest,Reps,ThermChan,fN1)

The following program shows the use of the CDM_PanelTemp instruction.

```
Public PTemp

DataTable (Test,True,-1)
DataInterval (0,1,Min,10)
Sample (1,PTemp,FP2)
EndTable

BeginProg
Scan (1,Sec,3,0)
CDM_PanelTemp( CDM_A108,1,PTemp,1,1,15000)
calltable (Test)
NextScan
EndProg
```

## Remarks

A measurement from the CDM_PanelTemp instruction can be used as the reference temperature for one or more thermocouple measurements. The CDM devices have multiple thermistors built into the wiring panel. If using this instruction for a thermocouple measurement, choose the thermister closest to the analog channel on which you wish to make the thermocouple measurement.

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

For the CDM_PanelTemp instruction, measurements are made on consecutive channels. If the Reps parameter is greater than 1, the Dest parameter must be a variable array.

# ThermChan (Thermistor Channel)

Used to select the thermistor that will be used for the measurement. Select the ThermChan that is closest to the analog measurement you are making. If the Reps parameter is >1, ThermChan will be incremented by 1 for each rep. Valid channels will vary depending upon the CDM being used. Right-click the parameter to display a drop-down list box.

| Code | Description             |
| ---- | ----------------------- |
| 1    | Terminals 1 through 8   |
| 2    | Terminals 9 through 16  |
| 3    | Terminals 17 through 24 |
| 4    | Terminals 25 through 32 |

Type: Constant

# fN1 (First Notch Frequency)

Determines the lowest frequency that will be eliminated or notched out by the sinc filter. This filter notches out frequencies at integer multiples of fN1by averaging for a time equal to 1/fN1; thus, lower fN1frequencies result in longer measurement times. Any value between 2.5 Hz and 30,000 Hz can be entered, but the value entered will be rounded to the nearest of the frequencies below. The three most common noise filtering options are listed first.

| Option | Description                                                       |
| ------ | ----------------------------------------------------------------- |
| 30000  | Performs a 0.0333 millisecond integration (for fast measurements) |
| 60     | Performs a 16.67 millisecond integration (filters 60 Hz noise)    |
| 50     | Performs a 20 millisecond integration (filters 50 Hz noise)       |
| 15000  | Performs a 0.0667 millisecond integration                         |
| 7500   | Performs a 0.0133 millisecond integration                         |
| 3750   | Performs a 0.2667 millisecond integration                         |
| 2000   | Performs a 0.5 millisecond integration                            |
| 1000   | Performs a 1 millisecond integration                              |
| 500    | Performs a 2 millisecond integration                              |
| 100    | Performs a 10 millisecond integration                             |
| 30     | Performs a 33.33 millisecond integration                          |
| 25     | Performs a 40 millisecond integration                             |
| 15     | Performs a 66.67 millisecond integration                          |
| 10     | Performs a 100 millisecond integration                            |
| 5      | Performs a 200 millisecond integration                            |
| 2.5    | Performs a 400 millisecond integration                            |

Type: Constant
