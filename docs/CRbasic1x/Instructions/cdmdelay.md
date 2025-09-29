# CDM_Delay (CDM Delay)

The CDM_Delay instruction is used to delay the program running in aCDM **Note:** for a specified period of time. For additional information on controller area network peripherals and Campbell modules, see [CPI and CDM](https://www.campbellsci.com/news-cpi-cdm).

## Syntax

```
CDM_Delay
```

(CDMType,CPIAddress,Option,Delay,Units)

This example uses the CDM_Delay instruction to delay the program for 500 milliseconds after a SW12 port is turned on and before a voltage measurement is made.

```
Public Volt1

'Define Data Tables
DataTable (FiveSec,1,-1)'Set table size to # of records, or -1 to autoallocate.
DataInterval (0,5,Sec,10)
Sample (1,Volt1,FP2)
EndTable

BeginProg
Scan (1,Sec,3,0)
CDM_SW12 (VOLT116,1,1,1 ,0)
CDM_Delay(VOLT116,1,0,500,mSec)
CDM_VoltDiff (VOLT116,1,Volt1,1,mv5000C,1,True,200,15000,1.0,0)
CDM_SW12 (VOLT116,1,1,0,0)
CallTable FiveSec
NextScan
EndProg
```

## Remarks

The CDM_Delay instruction is used to delay the measurement task sequence or the processing instructions for the time period specified by the Delay and Units arguments, before progressing to the next measurement or processing instruction.

The Scan Interval should be sufficiently long to process all measurements plus the delay period. If the delay is applied to the measurement task sequence and the scan interval is not long enough to process all measurements plus the delay, the program will not compile when sent to the datalogger. If the delay is applied to the processing task sequence, the program will compile but scans will be skipped.

Note that the CDM_Delay instruction with option set to 0 only delays the addressed device. In other words, each device that needs a delay must have its own CDM_Delay instruction.

The CDM_Delay instruction with option set to 1 essentially makes the CDM_Delay instruction equivalent to the datalogger s Delay instruction since the processing delay happens at the datalogger.

## Parameters

# CDMType (CDM Type)

Used to specify the type ofCDM **Note:** used by the instruction. This is a read-only setting in the device which cannot be changed. Right-click the parameter to display a list of valid CDM types.

Type: Constant

# CPIAddress (CPI Address)

TheCPI

** Note:**CPI is a proprietary interface for communications between Campbell Scientific dataloggers and Campbell Scientific CDM peripheral devices. It consists of a physical layer definition and a data protocol. CDM devices are similar to Campbell Scientific SDM devices in concept, but the use of the CPI bus enables higher data-throughput rates and use of longer cables. CDM devices require more power to operate in general than do SDM devices.

address configured in theCDM ** Note:** module. Valid range is 1 through 120. Note that the CPIAddress cannot be a variable because the datalogger must know all information for a given CDM module at program compile.(e.g., before the program runs).

Type: Constant integer

# Option

Determines whether the delay should apply to the measurement task sequence or the processing instructions. Right-click the parameter to display a drop-down list box.

| Code | Description                                                                                                                                                                                                           |
| ---- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 0    | Delay will affect the measurement task sequence. Processing will continue to take place as needed in the background. When this option is chosen, the Delay instruction must not be placed in a conditional statement. |
| 1    | Delay will affect processing. Measurements will continue as called for by the task sequencer.                                                                                                                         |

Type: Constant

# Delay

The time to delay the program at the designated place (10 microsecond resolution). The time units are selected by the Units argument.

Type: Constant (or expression that evaluates as a constant).

# Units

For the Delay instruction, the Units parameter is the time units for the delay. A numeric code is entered. Right-click the parameter to display a list.

| Code | Alphanumeric Code | Description  |
| ---- | ----------------- | ------------ |
| 0    | usec              | microseconds |
| 1    | msec              | milliseconds |
| 2    | sec               | seconds      |
| 3    | min               | minutes      |
| 4    | hr                | hours        |
| 5    | day               | days         |

Type: Constant
