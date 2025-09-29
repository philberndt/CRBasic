# CDM_Battery (CDM Power Supply Voltage)

The CDM_Battery instruction is used to read and return the voltage of the power supply for aCDM **Note:** . For additional information on controller area network peripherals and Campbell modules, see [CPI and CDM](https://www.campbellsci.com/news-cpi-cdm).

## Syntax

```
CDM_Battery
```

(CDMType,CPIAddress,Dest)

The following program example makes a simple battery measurement in a CDM-A116 and stores the 24 hour minimum voltage, along with the time the minimum occurred, to a data table.

```
Public CDM_BattV

DataTable (CDMBatMin,True,9999 )
DataInterval (0,1,Day,10)
Minimum (1,CDM_BattV,FP2,False,True)
EndTable

BeginProg
Scan (1,Sec,3,0)
CDM_Battery(CDM_A116,1,CDM_BattV)
CallTable CDMBatMin
NextScan
EndProg
```

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

** WARNING:**If this instruction is placed inside a conditional statement, the datalogger will attempt to compile the program in Sequential mode. If the program is forced into PipeLine mode using the PipelineMode instruction, compilation will fail.
