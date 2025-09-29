# CDM_PulsePort (CDM Toggle Port State)

The CDM_PulsePort instruction is used to toggle the state of a digital channel on aCDM **Note:** , delay, and then toggle the channel again. For additional information on controller area network peripherals and Campbell modules, see [CPI and CDM](https://www.campbellsci.com/news-cpi-cdm).

## Syntax

CDM_PulsePort(CDMType,CPIAddress,Port,Delay)

The following program shows the use of the CDM_PulsePort instruction to pulse port SW5-1 on a CDM-A108.

```
BeginProg
Scan (1,Sec,3,0)

CDM_PulsePort(CDM_A108,1,1,10)

NextScan
EndProg
```

## Remarks

This instruction toggles a digital channel (or "port"), delays the specified amount of time, toggles the channel, and then delays a second time. The second delay in the instruction allows it to be used to create a 50 percent duty cycle clock for clocking multiplexers.

## Parameters

# CDMType (CDM Type)

Used to specify the type ofCDM **Note:** used by the instruction. This is a read-only setting in the device which cannot be changed. Right-click the parameter to display a list of valid CDM types.

Type: Constant

# CPIAddress (CPI Address)

TheCPI

** Note:**CPI is a proprietary interface for communications between Campbell Scientific dataloggers and Campbell Scientific CDM peripheral devices. It consists of a physical layer definition and a data protocol. CDM devices are similar to Campbell Scientific SDM devices in concept, but the use of the CPI bus enables higher data-throughput rates and use of longer cables. CDM devices require more power to operate in general than do SDM devices.

address configured in theCDM ** Note:** module. Valid range is 1 through 120. Note that the CPIAddress cannot be a variable because the datalogger must know all information for a given CDM module at program compile.(e.g., before the program runs).

Type: Constant integer

# SW5Port (Switched 5V Port)

The switched 5 volt port to use with this instruction. The number of available ports depends upon the CDM being programmed. Right-click the parameter for a list of valid options.

| Code | Description   |
| ---- | ------------- |
| 1    | Switched 5V 1 |
| 2    | Switched 5V 2 |
| 3    | Switched 5V 3 |
| 4    | Switched 5V 4 |

Type: Constant

# Delay

The amount of time, in microseconds, to delay after toggling the port. The delay is used after both the first and second toggles of the port.

Type: Constant
