# CPISpeed (CPI Communication Speed)

The CPISpeed instruction is used to set the bit rate that the datalogger will use for communication with the devices on the CPI bus.

## Syntax

CPISpeed(BitRate)

```
'Declare Variables
Public Temp(16), DestBR

'Define Data Tables
DataTable (Test,1,-1)'Set table size to # of records, or -1 to autoallocate.
DataInterval (0,1,Sec,10)
Sample (1,DestBR,FP2)
Sample (1,Temp,FP2)
EndTable

'Main Program
BeginProg
CPISpeed(1000)'fast CPI
Scan (5,mSec,200,0)
CDM_TCComp (TEMP120,1,Temp,16,1,TypeT,1,0)
CallTable Test
NextScan
EndProg
```

## Remarks

If CPISpeed is not in the datalogger program, a default bit rate of 250 kbps is used.

## Parameter

# BitRate (Bit Rate)

Specifies the bit rate in kbps that is used for communication on theCPI

**Note:** CPI is a proprietary interface for communications between Campbell Scientific dataloggers and Campbell Scientific CDM peripheral devices. It consists of a physical layer definition and a data protocol. CDM devices are similar to Campbell Scientific SDM devices in concept, but the use of the CPI bus enables higher data-throughput rates and use of longer cables. CDM devices require more power to operate in general than do SDM devices.

bus. Valid entries are 50, 125, 250, 500, and 1000. Right-click the parameter to display a list of options.

Type: Constant
