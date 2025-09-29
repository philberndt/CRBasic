# PulsePort (Toggle Port State)

The PulsePort instruction is used to toggle the state of a port, delay, and then toggle the port again.

## Syntax

PulsePort(Port,Delay)

The following program shows the use of the PulsePort instruction to measure 10 thermocouples attached to an AM16/32 multiplexer.

```
'CR300
'Declare Variables and Units
Dim LCount_4
Public Batt_Volt
Public RefTemp
Public Temp_C(10)

Units Batt_Volt=Volts
Units RefTemp=Deg C
Units Temp_C=Deg C

'Define Data Tables
DataTable(Table1,True,-1)
DataInterval(0,60,Min,0)
Sample(10,Temp_C(1),FP2)
EndTable

'Main Program
BeginProg
Scan(30,Sec,1,0)
'Default Datalogger Battery Voltage measurement Batt_Volt:
Battery(Batt_Volt)
'107 Temperature Probe measurement RefTemp:
Therm107(RefTemp,1,1,Vx1,0,4000,1.0,0.0)

'Turn AM16/32 Multiplexer On
PortSet(C1,1)
LCount_4=1
SubScan(0,uSec,10)
'Switch to next AM16/32 Multiplexer channel
PulsePort(C2,10000)
'Type T (copper-constantan) Thermocouple measurements Temp_C on the AM16/32Multiplexer:
TCDiff(Temp_C(LCount_4),1,mv34,2,TypeT,RefTemp,True,0,4000,1,0)

LCount_4=LCount_4+1
NextSubScan
'Turn AM16/32 Multiplexer Off
PortSet(C1,0)
'Call Data Table and Store Data
CallTable(Table1)
NextScan
EndProg
```

## Remarks

This instruction toggles a port, delays the specified amount of time, toggles the port, and then delays a second time. The second delay in the instruction allows it to be used to create a 50 percent duty cycle clock for clocking multiplexers.

## Parameters

# Port

The number of the port to use in this instruction. An alphanumeric code is entered. Right-click to display a list.

| Code | Description            | High (V) |
| ---- | ---------------------- | -------- |
| C1   | Control Port 1         | ~5       |
| C2   | Control Port 2         | ~5       |
| SE1  | Single ended channel 1 | ~3.3     |
| SE2  | Single ended channel 2 | ~3.3     |
| SE3  | Single ended channel 3 | ~3.3     |
| SE4  | Single ended channel 4 | ~3.3     |
| P_SW | Pulse terminal (P_SW)  | ~3.3     |

Type: Constant

# Delay

The amount of time, in microseconds, to delay after toggling the port. The delay is used after both the first and second toggles of the port.

Type: Constant
