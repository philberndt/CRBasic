# CPIAddModule (Configure Device for CPI Network)

The CPIAddModule instruction is used to programmatically configure a device for use in aCPI

**Note:** CPI is a proprietary interface for communications between Campbell Scientific dataloggers and Campbell Scientific CDM peripheral devices. It consists of a physical layer definition and a data protocol. CDM devices are similar to Campbell Scientific SDM devices in concept, but the use of the CPI bus enables higher data-throughput rates and use of longer cables. CDM devices require more power to operate in general than do SDM devices.

network. For additional information on controller area network peripherals and Campbell modules, see [CPI and CDM](https://www.campbellsci.com/news-cpi-cdm).

## Syntax

CPIAddModule(CDMType,CDMSerialNo,CDMDeviceName,CPIAddress)

```
'The following example program shows the use of the CPIAddModule instruction to add a CDM-A108 and a CDM-A116 to the CPI network.

Public PTemp,Batt_volt,A108_BattV,A116_BattV,A108_PTemp,A116_PTemp

'Define Data Tables
DataTable (Test,1,-1)'Set table size to # of records, or -1 to autoallocate.
DataInterval (0,15,Sec,10)
Minimum(1,batt_volt,FP2,False,False)
Sample (1,Ptemp,FP2)
Sample (1,A108_BattV,FP2)
Sample (1,A116_BattV,FP2)
Sample (1,A108_PTemp,FP2)
Sample (1,A116_Ptemp,FP2)
EndTable

BeginProg

CPIAddModule( CDM_A108, 1608,"Autotest-A108",1)
CPIAddModule( CDM_A116, 1702,"Autotest-A116",2)

Scan(1,Sec,0,0)
PanelTemp(PTemp,15000)
Battery(Batt_volt)
CDM_Battery (CDM_A108,1,A108_BattV)
CDM_PanelTemp (CDM_A108,1,A108_PTemp,1,1,60)
CDM_Battery (CDM_A116,2,A116_BattV)
CDM_PanelTemp (CDM_A116,2,A116_PTemp,1,1,60)
CallTable Test
NextScan
EndProg
```

## Remarks

The CPIAddModule instruction is used to configure aCDM **Note:** module for use within theCPI

** Note:**CPI is a proprietary interface for communications between Campbell Scientific dataloggers and Campbell Scientific CDM peripheral devices. It consists of a physical layer definition and a data protocol. CDM devices are similar to Campbell Scientific SDM devices in concept, but the use of the CPI bus enables higher data-throughput rates and use of longer cables. CDM devices require more power to operate in general than do SDM devices.

network under program control. A separate instruction is used for each CDM module to be configured. CPIAddModule is not required to configure a device; rather, configuration can be done using Device Configuration utility, the keyboard display, or other software. However, if this instruction is in the running program, it will overwrite any configuration of CDMDeviceName or CPIAddress that may have previously been set by other means. Note that CDMType and CDMSerialNo are not user configurable parameters, but they must match the device type and serial number of the CDM module to be configured. Also, if CPIAddModule is used in the running program, this prevents reconfiguration of the CDMDeviceName or CPIAddress via the CPIStatus table or keyboard display. In order to reconfigure the modules via the CPIStatus table or the keyboard display, the program running CPIAddModule must be stopped.

## Parameters

# CDMType (CDM Type)

Used to specify the type ofCDM ** Note:** used by the instruction. This is a read-only setting in the device which cannot be changed. Right-click the parameter to display a list of valid CDM types.

Type: Constant

# CDMSerialNo (CDM Serial Number)

Used to specify the serial number of CDM used by the instruction. This is a read-only setting in the device which cannot be changed.

Type: Constant

# CDMDeviceName (CDM Device Name)

User-defined string constant that can be used to help identify the device in theCPI

** Note:**CPI is a proprietary interface for communications between Campbell Scientific dataloggers and Campbell Scientific CDM peripheral devices. It consists of a physical layer definition and a data protocol. CDM devices are similar to Campbell Scientific SDM devices in concept, but the use of the CPI bus enables higher data-throughput rates and use of longer cables. CDM devices require more power to operate in general than do SDM devices.

network.

Type: String Constant

# CPIAddress (CPI Address)

TheCPI

** Note:**CPI is a proprietary interface for communications between Campbell Scientific dataloggers and Campbell Scientific CDM peripheral devices. It consists of a physical layer definition and a data protocol. CDM devices are similar to Campbell Scientific SDM devices in concept, but the use of the CPI bus enables higher data-throughput rates and use of longer cables. CDM devices require more power to operate in general than do SDM devices.

address configured in theCDM ** Note:** module. Valid range is 1 through 120. Note that the CPIAddress cannot be a variable because the datalogger must know all information for a given CDM module at program compile.(e.g., before the program runs).

Type: Constant integer
