# CDM_MuxSelect (CDM Multiplexer Channel Select)

The CDM_MuxSelect instruction is used to select a specified channel on an AM16/32A or AM16/32B multiplexer using aCDM **Note:** . For additional information on controller area network peripherals and Campbell modules, see [CPI and CDM](https://www.campbellsci.com/news-cpi-cdm).

## Syntax

CDM_MuxSelect(CDMType,CPIAddress,ClkPort,ResetPort,ClockPW,MuxChan,Mode)

The following program shows the use of the CDM_MuxSelect instruction to measure and control a multiplexer.

```
Public i
Public Meas(5)

BeginProg
i=1
Scan (1,Sec,3,0)
'other program instructions
CDM_MuxSelect(CDM_A108,1,1,2,10,4,1)' selects the mux to channel 4 in B Mode
SubScan (0,0,5)
CDM_VoltDiff(CDM_A108,1,Meas(i),1,mV5000,1,0,3000,250,1.0,0.0)
PulsePort(C1,5000)' advance the mux
i = i + 1
NextSubScan
PortSet(C2,0)'turn off mux
i=1
NextScan
EndProg
```

## Remarks

This instruction "wakes up" the multiplexer and clocks it to the specified starting channel. It can be used in conjunction with the SubScan and PulsePort instructions to step through the multiplexer's measurement channels to make measurements.

When measurements are completed, the ResetPort should be set low using PortSet to turn off the multiplexer (see example).

## Parameters

# CDMType (CDM Type)

Used to specify the type ofCDM **Note:** used by the instruction. This is a read-only setting in the device which cannot be changed. Right-click the parameter to display a list of valid CDM types.

Type: Constant

# CPIAddress (CPI Address)

TheCPI

** Note:**CPI is a proprietary interface for communications between Campbell Scientific dataloggers and Campbell Scientific CDM peripheral devices. It consists of a physical layer definition and a data protocol. CDM devices are similar to Campbell Scientific SDM devices in concept, but the use of the CPI bus enables higher data-throughput rates and use of longer cables. CDM devices require more power to operate in general than do SDM devices.

address configured in theCDM ** Note:** module. Valid range is 1 through 120. Note that the CPIAddress cannot be a variable because the datalogger must know all information for a given CDM module at program compile.(e.g., before the program runs).

Type: Constant integer

# ClkPort (Clock Port)

The switched 5V port that will be used to clock (ClkPort) or wake up and reset (ResetPort) the multiplexer. If multiple mutiplexers are used in the program, each must have a unique ResetPort. A numeric code is entered for this argument. Valid ports vary depending upon the CDMType. Right-click the parameter for a drop-down list of valid options.

Type: Constant

# ResetPort (Reset Port)

The switched 5V port that will be used to clock (ClkPort) or wake up and reset (ResetPort) the multiplexer. If multiple mutiplexers are used in the program, each must have a unique ResetPort. A numeric code is entered for this argument. Valid ports vary depending upon the CDMType. Right-click the parameter for a drop-down list of valid options.

Type: Constant

# ClockPW (Clock Period)

Used to control the period of the clock used to advance the multiplexer. The value is entered in milliseconds.

Type: Constant

# MuxChan (Multiplexer Channel)

Specifies the first measurement channel on the multiplexer. It can be a constant or a variable.If programmed as a variable, the datalogger program will be forced into SequentialMode at compile time.

Type: Constant or variable

# Mode

Specifies what type of clocking will be used. Enter 0 for AM16/32A clocking and 1 for AM16/32B clocking.

Type: Constant
