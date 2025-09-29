# CSAT3BMonitor (Retrieve Data from CSAT3B or CSA3BH)

The CSAT3BMonitor instruction is used to retrieve auxiliary information from a CSAT3B or CSAT3BH sonic anemometer.

## Syntax

CSAT3BMonitor(Dest,Bus,Address)

The following example program measures a CSAT3B sonic anemometer using aCR1000Xdatalogger. Programming for other dataloggers is similar.

```
'CR1000X Series Datalogger

'*** Wiring ***
'SDM INPUT (CSAT3B SDM address = 3)
'C1 CSAT3B SDM Data (green)
'C2 CSAT3B SDM Clock (white)
'C3 CSAT3B SDM Enable (brown)
'G CSAT3B SDM reference (black)
' CSAT3B SDM shield (clear)

'POWER IN
'12V datalogger (red)
'G datalogger (black)

'EXTERNAL POWER SUPPLY
'POS CSAT3B power (red)
' datalogger (red)
'NEG CSAT3B power reference (black)
' CSAT3B power shield (clear)
' datalogger (black)

PipeLineMode

'---------------------------------------------
' Define Constants, Variables, and Aliases
'---------------------------------------------
Const SDM_ADDR = 3'SDM Address of Device
Public CSATVals(5)
Public CSATMonitorVals(4)
Alias CSATVals(1) = Ux
Alias CSATVals(2) = Uy
Alias CSATVals(3) = Uz
Alias CSATVals(4) = SonTemp
Alias CSATVals(5) = Diag
Alias CSATMonitorVals(1) = BoardTemp
Alias CSATMonitorVals(2) = BoardHumidity
Alias CSATMonitorVals(3) = InclinePitch
Alias CSATMonitorVals(4) = InclineRoll

'---------------------------------------------
' Define Data Tables
'---------------------------------------------
DataTable (SonicData,1,-1)
Sample (5,CSATVals(1),IEEE4)
EndTable

DataTable (MonitorData,1,-1)
DataInterval (0,5,Sec,10)

Sample (4,CSATMonitorVals(1),IEEE4)
EndTable

'---------------------------------------------
' Main Program
'---------------------------------------------
BeginProg
Scan(50,msec,500,0)'20 Hz Scan
'CSAT3B(Destination, Bus, Address, OperatingMode)
CSAT3B(CSATVals(),0,SDM_ADDR,0)

CallTable(SonicData)
NextScan

SlowSequence

Scan(5,sec,0,0)'5 second scan
'CSAT3BMonitor (Destination, Bus, Address)
CSAT3BMonitor(CSATMonitorVals(),0,SDM_ADDR)

CallTable(MonitorData)
NextScan
EndProg
```

## Remarks

The CSAT3BMonitor instruction retrieves the internal temperature and relative humidity of the electronics enclosure, and also the inclination of the anemometer. This instruction should be placed within a SlowSequence in the datalogger program.

## Parameters

# Dest (Destination Variable)

A variable array that stores the values returned by the anemometer. It must be declared as a float with at least 4 elements in the array. The sensor returns the following data in response to this instruction:

- **Dest(1)**: Electronics temperature in degrees C

- ** Dest(2)**: Electronics RH as a percent

- ** Dest(3)**: Inclinometer pitch in degrees

- ** Dest(4)**: Inclinometer roll in degrees

Type: Variable array

# Bus

Determines whether communication with the CSAT3B is via theSDM

** Note:**Synchronous Device for Measurement. A processor-based peripheral device or sensor that communicates with the datalogger via hardwire over a short distance using a protocol proprietary to Campbell Scientific.

orCPI

** Note:**CPI is a proprietary interface for communications between Campbell Scientific dataloggers and Campbell Scientific CDM peripheral devices. It consists of a physical layer definition and a data protocol. CDM devices are similar to Campbell Scientific SDM devices in concept, but the use of the CPI bus enables higher data-throughput rates and use of longer cables. CDM devices require more power to operate in general than do SDM devices.

bus. Enter ** 0**for SDM or ** 1**for CPI.

Type: Constant

# Address (Address of CSAT3B)

Identifies the unique address of the CSAT3B on the communication bus. ForSDM

** Note:**Synchronous Device for Measurement. A processor-based peripheral device or sensor that communicates with the datalogger via hardwire over a short distance using a protocol proprietary to Campbell Scientific.

, valid addresses are 1 through 14. ForCPI

** Note:**CPI is a proprietary interface for communications between Campbell Scientific dataloggers and Campbell Scientific CDM peripheral devices. It consists of a physical layer definition and a data protocol. CDM devices are similar to Campbell Scientific SDM devices in concept, but the use of the CPI bus enables higher data-throughput rates and use of longer cables. CDM devices require more power to operate in general than do SDM devices.

, valid addresses are 1 through 120.

Type: Constant
