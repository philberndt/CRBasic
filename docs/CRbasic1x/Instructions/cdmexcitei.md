# CDM_ExciteI (CDM Current Excitation)

The CDM_ExciteI instruction is used to enable a current excitation channel on aCDM **Note:** . For additional information on controller area network peripherals and Campbell modules, see [CPI and CDM](https://www.campbellsci.com/news-cpi-cdm).

## Syntax

CDM_ExciteI(CDMType,CPIAddress,IxChan,IxuA,Delay)

The following portion of code demonstrates the use of the CDM_ExciteI instruction to apply a current to a current excitation channel.

```
Public Tref, Tblk1, ExmA

DataTable (Main,True,-1)
Sample (1,Tblk1,FP2)
EndTable

BeginProg
Scan(1,Sec,1,0)
CDM_PanelTemp (CDM_A108,1,Tref,1,1,15000)
CDM_TCDiff(CDM_A108,1,Tblk1,1,mV200,1,TypeT, Tref,0,0,60,1.8,32)
If TBlk1 > 77 Then
ExmA = 2000'2A if greater than 77 degrees
Else
ExmA = 1000'1A if less than 77 degrees
EndIf
CDM_ExciteI(CDM_A108,1,X1,ExmA,10)'Set channel; delay 10 ms
CallTable MAIN
NextScan
EndProg
```

## Remarks

This instruction is used to set to the current level (μAmperes) for a specified current excitation channel.

## Parameters

# CDMType (CDM Type)

Used to specify the type ofCDM **Note:** used by the instruction. This is a read-only setting in the device which cannot be changed. Right-click the parameter to display a list of valid CDM types.

Type: Constant

# CPIAddress (CPI Address)

TheCPI

** Note:**CPI is a proprietary interface for communications between Campbell Scientific dataloggers and Campbell Scientific CDM peripheral devices. It consists of a physical layer definition and a data protocol. CDM devices are similar to Campbell Scientific SDM devices in concept, but the use of the CPI bus enables higher data-throughput rates and use of longer cables. CDM devices require more power to operate in general than do SDM devices.

address configured in theCDM ** Note:** module. Valid range is 1 through 120. Note that the CPIAddress cannot be a variable because the datalogger must know all information for a given CDM module at program compile.(e.g., before the program runs).

Type: Constant integer

# IxChan (Excitation Channel)

The excitation channel (X1, X2, etc.) to use for the first measurement. The number of available channels depends upon the CDM being programmed. Right-click the parameter for a list of valid options.

Type: Constant

This is a snippet

# IxUA (Current Excitation in Amps)

The current excitation, in microAmps, to apply to the excitation channel. The allowable range is 2500 A. For ExciteI() only, with DiffEx set to 1, the allowable range is 12500 A.

Type: Constant

# Delay

The amount of time, in microseconds, to delay before moving on to the next instruction. Excitation is turned off after the specified delay. If the Delay is set to 0 (default), the datalogger will apply the desired excitation and immediately move to the next instruction without turning excitation off. The excitation will be held until the end of the program scan or until another instruction sets an excitation.

Type: Constant (or expression that evaluates as a constant)

** Caution:**Do NOT place this instruction inside a conditional statement when running inpipeline mode

** Note:**A CRBasic program execution mode wherein instructions are evaluated in groups of like instructions, with a set group prioritization.

.
