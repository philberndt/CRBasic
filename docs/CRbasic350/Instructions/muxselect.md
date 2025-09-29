# MuxSelect (Multiplexer Channel Select)

The MuxSelect instruction is used to select a specified channel on an AM16/32A or AM16/32B multiplexer.

## Syntax

MuxSelect(ClkPort,ResetPort,ClockPW,MuxChan,Mode)

The following program shows the use of the MuxSelect instruction to measure and control a multiplexer.

```
'Multiplexer in 4x16 Mode
'Declare Variables and Units
Public SEVolt(9)
Units SEVolt=mV
'Define Data Tables
DataTable (Hourly,True,-1)
DataInterval (0,60,Min,10)
Sample (9,SEVolt(),FP2,False)
EndTable

DataTable (Daily,True,-1)
DataInterval (0,1440,Min,10)
Average (9,SEVolt(),FP2,False)
EndTable

'Main Program
BeginProg
SW12 (SW12_1,1)'provide power to AM16/32B
'Main Scan
Scan(30,Sec,1,0)
'>>>>>>>>> Set 1
'Turn AM16/32B Multiplexer On, start measurements on mux channel 1
MuxSelect(C1,C2,20,1,1)
'3 repetitions, writing to SEVolt(1), SEVolt(2) and SEVolt(3)
'3 repetitions, measuring 1H, 1L, 2H on mux
VoltSe(SEVolt(1),3,mv2500,1,True,0,4000,1,0)

'>>>>>> Set 2
PulsePort (C1 ,10000)'to move to Set 2
'start measurements on mux channel 3
'3 repetitions, writing to SEVolt(4), SEVolt(5) and SEVolt(6)
'3 repetitions, measuring 3H, 3L, 4H on mux
VoltSe (SEVolt(4),3,mv2500,1,True,0,4000,1,0)

'>>>>>> Set 3
PulsePort (C1 ,10000)'to move to Set 3
'3 repetitions, writing to SEVolt(7), SEVolt(8) and SEVolt(9)
'3 repetitions, measuring 5H, 5L, 6H on mux
VoltSE (SEVolt(7),3,mv2500,1,True,0,4000,1,0)

'Turn AM16/32B Multiplexer Off
PortSet (C2,0)
'Call Data Tables and Store Data
CallTable Hourly
CallTable Daily
NextScan
EndProg
```

## Remarks

This instruction "wakes up" the multiplexer and clocks it to the specified starting channel. It can be used in conjunction with the [SubScan](subscannextsubscan.md) and [PulsePort](pulseport.md) instructions to step through the multiplexer's measurement channels to make measurements.

When measurements are completed, the ResetPort should be set low using [PortSet](portset.md) to turn off the multiplexer (see example).

## Parameters

# ClkPort (Clock Port)

The port used to clock the multiplexer (for example, advance its channel). One clock port may be used to clock several devices. An alphanumeric code is entered for this argument.

| Code | Description            | High (V) | Maximum Current Source |
| ---- | ---------------------- | -------- | ---------------------- |
| C1   | Control Port 1         | ~5       | 10 mA at 3.5 V         |
| C2   | Control Port 2         | ~5       | 10 mA at 3.5 V         |
| SE1  | Single ended channel 1 | ~3.3     | 100 A at 3.0 V         |
| SE2  | Single ended channel 2 | ~3.3     | 100 A at 3.0 V         |
| SE3  | Single ended channel 3 | ~3.3     | 100 A at 3.0 V         |
| SE4  | Single ended channel 4 | ~3.3     | 100 A at 3.0 V         |
| VX1  | Excitation channel 1   | ~5       | 50 mA at 5 V           |
| VX2  | Excitation channel 2   | ~5       | 50 mA at 5 V           |

Type: Constant

# ResPort (Reset Port)

The control port that will be used to wake up and reset the multiplexer. If multiple multiplexers are used in the program, each multiplexer must have its own unique ResetPort. An alphanumeric code is entered for this argument. Right-click to display a list.

| Code | Description            |
| ---- | ---------------------- |
| C1   | Control Port 1         |
| C2   | Control Port 2         |
| SE1  | Single ended channel 1 |
| SE2  | Single ended channel 2 |
| SE3  | Single ended channel 3 |
| SE4  | Single ended channel 4 |
| VX1  | Excitation channel 1   |
| VX2  | Excitation channel 2   |

Type: Constant

# ClockPW (Clock Period)

Used to control the period of the clock used to advance the multiplexer. The value is entered in milliseconds.

Type: Constant

# MuxChan (Multiplexer Channel)

Specifies the first measurement channel on the multiplexer. It can be a constant or a variable.

Type: Constant or variable

# Mode

Specifies what type of clocking will be used. Enter 0 for AM16/32A clocking and 1 for AM16/32B clocking.

Type: Constant
