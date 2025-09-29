# ExciteV (Set Voltage Excitation)

The ExciteV instruction is used toset an excitation channel to a specified value.

## Syntax

ExciteV(ExChan,ExmV,Delay)

In the following program, an excitation channel is excited prior to making a voltage measurement.

```
Public Volt1

DataTable (Test1,True,-1)
DataInterval (0,1,Min,10)
Sample (1,Volt1,FP2)
EndTable

BeginProg
Scan (1,Sec,3,0)
ExciteV(Vx1,2500,0)'Apply excitation
'You may need to add delay prior to measurement.
'This can be done by increasing the settling time parameter in the measurement instruction,
'or by using the Delay() instruction prior to measurement.

VoltDiff (Volt1,1,mv5000C,1,True ,2000,15000,1.0,0)
ExciteV(Vx1,0,0)'turn off excitation
CallTable Test1
NextScan
EndProg
```

The following code shows how to use the Delay parameter to create a 10 ms pulse to the specified port.

```
BeginProg
Scan (1,Sec,3,0)
ExciteV(Vx1,2500,10000)'Excite 10mS pulse
'Other Code
NextScan
EndProg
```

## Remarks

This instruction is used to set to the voltage excitation level (in millivolts) for a specifiedvoltage excitation channel. The Delay parameter is used to specify the length of time the excitation channel is enabled, after which, thechannelis set low and the datalogger moves on to the next instruction. If the Delay is set to 0, the excitation channel will be enabled and the voltage will be held until the end of the program scan, until another instruction sets an excitation voltage,**or until the instruction is interrupted by a measurement**. When the measurement is finished the excitation channel is returned to the value that was set prior to the measurement. Instructions that will not return the excitation to its former state are: PanelTemp, AM25T (if measuring the temp on the AM25T), Calibrate, Therm107, 108, and 109, and all bridge measurements.

Note that any time the measurement hardware is accessed when ExciteV is exciting a channel, the excitation will be turned off for the measurement and then turned back on. This includes when the MeasOff parameter of a subsequent voltage measurement instruction is set to True (and thus a measurement is made), or when an ExciteV resides in a slow sequence scan and the main scan makes a measurement. This interruption in ExciteV may adversely affect the sensor measurement. In general, the use of ExciteV in a slow sequence scan should be avoided.

## Parameters

# ExChan (Excitation Channel)

Specifies theexcitationterminal to use to excite the first measurement. If the Reps parameter is greater than 1, the excitation terminal used for subsequent measurements will be incremented with each measurement based on the MeasPEx parameter.

An alphanumeric code is entered. Right-click within the parameter to display a list.

| Alphanumeric | Description          |
| ------------ | -------------------- |
| VX1          | Excitation channel 1 |
| VX2          | Excitation channel 2 |
| VX3          | Excitation channel 3 |
| VX4          | Excitation channel 4 |

Type: Constant

# ExmV (Excitation Voltage in mV)

The excitation, in millivolts, to apply to the excitation channel. The allowable range is4000 mV.

Type: Constant. For ExciteV() only, this parameter can also be a Variable.

# Delay

The amount of time, in microseconds, to delay before moving on to the next instruction. Excitation is turned off after the specified delay. If the Delay is set to 0 (default), the datalogger will apply the desired excitation and immediately move to the next instruction without turning excitation off. The excitation will be held until the end of the program scan or until another instruction sets an excitation.

Type: Constant (or expression that evaluates as a constant)
