# SDMSpeed (SDM Speed)

The SDMSpeed instruction is used to change the bit period that the datalogger uses to clock the SDM data. Slowing down the clock rate may be necessary when long cable lengths are used to connect the datalogger and SDM devices.

## Syntax

SDMSpeed(BitPeriod)

The following program uses the SDMSpeed instruction to change the bit period to 30 prior to making measurements.

```
'Declare Variables
Public wind(5)
Public irga(5)
Alias wind(1) = Ux
Alias wind(2) = Uy
Alias wind(3) = Uz
Alias wind(4) = Ts
Alias wind(5) = sonic_diag
Alias irga(1) = CO2
Alias irga(2) = H2O
Alias irga(5) = press
Alias irga(4) = irga_diag
Units wind = m/s
Units Ts = C
Units sonic_diag = arb
Units CO2 = umol/mol
Units H2O = mmol/mol
Units press = kPa
Units irga_diag = arb

DataTable (ts_data,TRUE,-1)
DataInterval (0,0,mSec,10)
CardOut(0,-1)
Sample (5,Ux,IEEE4)
Sample (4,CO2,IEEE4)
EndTable

BeginProg
SDMSpeed(30)
Scan (50,mSec,3,0)
CSAT3 (Ux,1,3,91,20)
CS7500 (CO2,1,7,6)
CO2 = CO2*44
H2O = H2O*0.018
CallTable ts_data
NextScan
EndProg
```

## Parameter

# BitPeriod

Bit period defaults to 26 μsec (if the SDMSpeed instruction is not in the program). It has a minimum of 9 μsec, a max of 2 msec, and will be set to:

Actual bit_period (in μsec) = INT (BitPeriod parameter).

Type: Constant (or expression that evaluates as a constant) or Variable (in μsec)
