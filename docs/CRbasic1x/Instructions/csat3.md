# CSAT3 (Control and Retrieve Data from CSAT3)

The CSAT3 instruction is used to control and retrieve data from a Campbell Scientific CSAT3 3D sonic anemometer usingSDM

**Note:** Synchronous Device for Measurement. A processor-based peripheral device or sensor that communicates with the datalogger via hardwire over a short distance using a protocol proprietary to Campbell Scientific.

communications.

## Syntax

CSAT3(Dest,Reps,SDMAddress,Command,Option)

The following example measures a CSAT3 and a CS7500 (LI-7500).

```
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
CardOut (0,-1)

Sample (5,Ux,IEEE4)
Sample (4,CO2,IEEE4)
EndTable

BeginProg
SDMSpeed (30)
Scan (50,mSec,3,0)
CSAT3(Ux,1,3,91,20)
CS7500 (CO2,1,7,6)
CO2 = CO2*44
H2O = H2O*0.018

CallTable ts_data
NextScan
EndProg
```

## Remarks

Refer to the [CSAT3 product manual](https://s.campbellsci.com/documents/us/manuals/csat3.pdf) for complete information on this instruction.

## Parameters

# Dest (Destination Variable)

The datalogger variable name to receive the CSAT3 data. This variable must be dimensioned to a length of five to hold the CSAT3 Ux, Uy, Uz, speed of sound and diagnostic data. Right-click the parameter to display a list of defined variables.

# Reps (Repetitions)

The number of repetitions for the measurement or instruction.

Type: Constant integer (or expression that evaluates as a constant).

For the CSAT3 instruction, measurements are made on consecutive channels. If the Reps parameter is greater than 1, the Dest parameter must be a variable array.

# SDMAddress (Address of Device)

Defines the address of the device with which to communicate. ValidSDM

**Note:** Synchronous Device for Measurement. A processor-based peripheral device or sensor that communicates with the datalogger via hardwire over a short distance using a protocol proprietary to Campbell Scientific.

addresses are 0 through 14. Address 15 is reserved for the SDMTrigger instruction.

Some SDM instructions support repetitions. If a Reps parameter is present and it is greater than 1, the data logger will increment the SDM address used in the instruction for each subsequent device with which it communicates.

**NOTE:** CRBasic dataloggers use base 10 when addressing SDM devices. Edlog programmed dataloggers (e.g., CR10X, CR23X) used base 4 for addressing.[For more information, seeBase 10 to Base 4 Reference Chart.](../Info/base10tobase4referencechart.md)

# Command (CSAT3 Measurement Trigger)

Commands 90 - 92 send a measurement trigger to the CSAT3 with theSDM

**Note:** Synchronous Device for Measurement. A processor-based peripheral device or sensor that communicates with the datalogger via hardwire over a short distance using a protocol proprietary to Campbell Scientific.

address specified by the SDMAddress argument. The CSAT3 also sends data to the datalogger. Options 97 - 99 get data after a group trigger, SDMTrigger(), from the CSAT3 specified by the SDMAddress parameter without triggering a new CSAT3 measurements. The CSAT() instruction must be preceded by the SDMTrigger() instruction in order to use Options 97 - 99. Right-click the parameter to display a list.

| Code | Description                                                         |
| ---- | ------------------------------------------------------------------- |
| 90   | Trigger and get wind & speed of sound data.                         |
| 91   | Trigger and get wind & sonic temperature data.                      |
| 92   | Trigger and get wind & speed of sound data minus 340 m/s.           |
| 97   | Get wind & speed of sound data minus 340 m/s after a group trigger. |
| 98   | Get wind & sonic temperature data after a group trigger.            |
| 99   | Get wind & speed of sound data after a group trigger.               |

Type: Constant

# CSAT3 Option

The Option argument sets the CSAT3 s execution parameter. This parameter tells the CSAT3 which measurement parameters to use and what frequency to expect the measurement trigger from the datalogger. See the table below for a brief description of each of the parameter and the CSAT3 manual for a detailed description. Right-click the parameter to display a list.

| Code | Description                                               |
| ---- | --------------------------------------------------------- |
| 1    | Set execution parameter to 1 Hz                           |
| 2    | Set execution parameter to 2 Hz                           |
| 3    | Set execution parameter to 3 Hz                           |
| 5    | Set execution parameter to 5 Hz                           |
| 6    | Set execution parameter to 6 Hz                           |
| 10   | Set execution parameter to 10 Hz                          |
| 12   | Set execution parameter to 12 Hz                          |
| 15   | Set execution parameter to 15 Hz                          |
| 20   | Set execution parameter to 20 Hz                          |
| 30   | Set execution parameter to 30 Hz                          |
| 60   | Set execution parameter to 60 Hz                          |
| 61   | Set execution parameter to 60 Hz to 10 Hz oversample mode |
| 62   | Set execution parameter to 60 Hz to 20 Hz oversample mode |

Type: Constant
