# PortPairConfig (Set Port Pair Voltage)

The PortPairConfig instruction is used to set up the voltage for a pair of terminals on the datalogger.

## Syntax

PortPairConfig(Port,Option,PullOpt[optional] )

Following is a simple example of using PortPairConfig to set the voltage output for C1 to 3.3V. The scenario would be that the port is turned on to power a sensor that requires 3.3V, a delay occurs, the sensor is measured, and then the port is turned off. Note that C2 would also be set to 3.3V with this instruction, since the ports are configured in pairs.

```
Public Sensor

BeginProg

PortPairConfig(C1,2)

Scan (1,Sec,3,0)
PortSet(C1,1)
Delay (0,100,mSec)
'measure sensor
Portset (C1,0)
'Other programming instructions
NextScan
EndProg
```

## Remarks

By default, digital ports are set up for 5V. They can be set to 3.3V using this instruction. Ports are configured in terminal pairs if C1 is set to 3.3V using this instruction, C2 is also set to 3.3V.

## Parameters

# Port

Specifies the port to be configured using this instruction. Right click to display a drop-down list box of valid options.

| Alphanumeric | Description                        |
| ------------ | ---------------------------------- |
| C1           | Datalogger control terminals 1 & 2 |
| C3           | Datalogger control terminals 3 & 4 |
| C5           | Datalogger control terminals 5 & 6 |
| C7           | Datalogger control terminals 7 & 8 |

Type: Constant

# Option

Sets the voltage for the port. This parameter has the following options:

| Code | Description  |
| ---- | ------------ |
| 1    | 5V (default) |
| 2    | 3.3V         |

Type: Variable

## Optional Parameter

# PullOpt

Optional parameter that specifies the type of pull-up resistor to be used. If this parameter is left blank, the instruction defaults to 0 = pull down.

Type: Constant
