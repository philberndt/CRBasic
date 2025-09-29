# SDMTrigger (Trigger SDM Devices)

The SDMTrigger instruction is used to simultaneously trigger all the SDM devices that support group trigger.

## Syntax

SDMTrigger

### Example #1 (CSAT3)

```
PipeLineMode

'CSAT3 Variables
Public wind(4,5)
Alias wind(1,1) = Ux_1
Alias wind(1,2) = Uy_1
Alias wind(1,3) = Uz_1
Alias wind(1,4) = Ts_1
Alias wind(1,5) = diag_csat_1
Alias wind(2,1) = Ux_2
Alias wind(2,2) = Uy_2
Alias wind(2,3) = Uz_2
Alias wind(2,4) = Ts_2
Alias wind(2,5) = diag_csat_2
Alias wind(3,1) = Ux_3
Alias wind(3,2) = Uy_3
Alias wind(3,3) = Uz_3
Alias wind(3,4) = Ts_3
Alias wind(3,5) = diag_csat_3
Alias wind(4,1) = Ux_4
Alias wind(4,2) = Uy_4
Alias wind(4,3) = Uz_4
Alias wind(4,4) = Ts_4
Alias wind(4,5) = diag_csat_4
Units wind = m/s
Units Ts_1 = C
Units diag_csat_1 = arb
Units Ts_2 = C
Units diag_csat_2 = arb
Units Ts_3 = C
Units diag_csat_3 = arb
Units Ts_4 = C
Units diag_csat_4 = arb

BeginProg
Scan (100,mSec,3,0)
'Trigger all the CSAT3s to make measurements.
SDMTrigger
'Get all CSAT3 wind and sonic temperature data.
CSAT3 (Ux_1,4,3,98,10)
NextScan
EndProg
```

### Example #2 (CSAT3B)

```
PipeLineMode

'CSAT3B variables.
Public wind(4,5)
Alias wind(1,1) = Ux_1
Alias wind(1,2) = Uy_1
Alias wind(1,3) = Uz_1
Alias wind(1,4) = Ts_1
Alias wind(1,5) = diag_csat_1
Alias wind(2,1) = Ux_2
Alias wind(2,2) = Uy_2
Alias wind(2,3) = Uz_2
Alias wind(2,4) = Ts_2
Alias wind(2,5) = diag_csat_2
Alias wind(3,1) = Ux_3
Alias wind(3,2) = Uy_3
Alias wind(3,3) = Uz_3
Alias wind(3,4) = Ts_3
Alias wind(3,5) = diag_csat_3
Alias wind(4,1) = Ux_4
Alias wind(4,2) = Uy_4
Alias wind(4,3) = Uz_4
Alias wind(4,4) = Ts_4
Alias wind(4,5) = diag_csat_4
Units wind = m/s
Units Ts_1 = C
Units diag_csat_1 = arb
Units Ts_2 = C
Units diag_csat_2 = arb
Units Ts_3 = C
Units diag_csat_3 = arb
Units Ts_4 = C
Units diag_csat_4 = arb

BeginProg
Scan (100,mSec,3,0)
'Trigger all the CSAT3Bs to make measurements.
SDMTrigger
'Get all CSAT3B wind and sonic temperature data.
CSAT3B (Ux_1,0,3,0)
CSAT3B (Ux_2,0,4,0)
CSAT3B (Ux_3,0,5,0)
CSAT3B (Ux_4,0,6,0)
NextScan
EndProg
```

## Remarks

When the SDMTrigger instruction is executed, the datalogger sends a "measure now" group trigger to all connected SDM devices. Each device makes a measurement independently and sends the result back to the datalogger when a measurement instruction for that device is encountered in the program. Up to 15 group trigger devices can be connected to the SDM bus.
