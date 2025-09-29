# CDM_ExciteV (CDM Voltage Excitation)

The CDM_ExciteV instruction is used to apply a voltage excitation to an excitation channel on aCDM **Note:** . For additional information on controller area network peripherals and Campbell modules, see [CPI and CDM](https://www.campbellsci.com/news-cpi-cdm).

## Syntax

CDM_ExciteV(CDMType,CPIAddress,ExChan,ExmV,Delay)

In the following program, an excitation channel is excited prior to making a voltage measurement.

```
Public Volt1

DataTable (Test1,True,-1)
DataInterval (0,1,Min,10)
Sample (1,Volt1,FP2)

EndTable
BeginProg
Scan (1,Sec,3,0)
CDM_ExciteV(CDM_A108,1,X1,2500,200)
CDM_VoltDiff(CDM_A108,1,Volt1,1,mV5000,1,True ,0,60,1.0,0)
CallTable Test1
NextScan
EndProg
```

## Remarks

This instruction is used to set to the voltage excitation level (in millivolts) for a specified excitation channel. The Delay parameter is used to specify the length of time the excitation channel is enabled, after which, the terminal is set low and the datalogger moves on to the next instruction. If the Delay is set to 0, the excitation channel will be enabled and the voltage will be held until the end of the program scan, until another instruction sets an excitation voltage,**or until the instruction is interrupted by a measurement **. When the measurement is finished the excitation channel is returned to the value that was set prior to the measurement. Instructions that will not return the excitation to its former state are: CDM_PanelTemp, CDM_Therm107, 108, and 109, and all CDM bridge measurements.

Note that any time the measurement hardware is accessed when CDM_ExciteV is exciting a channel, the excitation will be turned off for the measurement and then turned back on. This includes when the MeasOff parameter of a subsequent voltage measurement instruction is set to True (and thus a measurement is made), or when a CDM_ExciteV resides in a slow sequence scan and the main scan makes a measurement. This interruption in CDM_ExciteV may adversely affect the sensor measurement. In general, the use of CDM_ExciteV in a slow sequence scan should be avoided.

## Parameters

# CDMType (CDM Type)

Used to specify the type ofCDM ** Note:** used by the instruction. This is a read-only setting in the device which cannot be changed. Right-click the parameter to display a list of valid CDM types.

Type: Constant

# CPIAddress (CPI Address)

TheCPI

** Note:**CPI is a proprietary interface for communications between Campbell Scientific dataloggers and Campbell Scientific CDM peripheral devices. It consists of a physical layer definition and a data protocol. CDM devices are similar to Campbell Scientific SDM devices in concept, but the use of the CPI bus enables higher data-throughput rates and use of longer cables. CDM devices require more power to operate in general than do SDM devices.

address configured in theCDM ** Note:** module. Valid range is 1 through 120. Note that the CPIAddress cannot be a variable because the datalogger must know all information for a given CDM module at program compile.(e.g., before the program runs).

Type: Constant integer

# ExChan (Excitation Channel)

Enter the excitation channel number (X1, X2...) used to excite the first measurement. The number of available channels depends upon the CDM being programmed. An alphanumeric code is entered. Right-click within the parameter to display a list of valid options.

Type: Constant

# ExmV (Excitation Voltage in mV)

Excitation, in millivolts, to apply to the sensor. The allowable range is5000 mV.

Type: Constant

# Delay

The amount of time, in microseconds, to delay before moving on to the next instruction. Excitation is turned off after the specified delay. If the Delay is set to 0 (default), the datalogger will apply the desired excitation and immediately move to the next instruction without turning excitation off. The excitation will be held until the end of the program scan or until another instruction sets an excitation.

Type: Constant (or expression that evaluates as a constant)

** Caution:**Do NOT place this instruction inside a conditional statement when running inpipeline mode

** Note:**A CRBasic program execution mode wherein instructions are evaluated in groups of like instructions, with a set group prioritization.

.
