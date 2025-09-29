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

VoltDiff (Volt1,1,mv2500,1,True ,2000,4000,1.0,0)
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

This instruction is used to set to the voltage excitation level (in millivolts) for a specified voltage excitation channel. If the delay parameter is 0, the voltage is not turned off at the end of the scan. Rather, the specified voltage will be held until it is turned off under program control **or until the instruction is interrupted by another measurement that uses the same excitation channel**. This continuous on behavior allows ExciteV to provide power to peripherals. Alternatively, the [SWVX()](SWVX.md) instruction may be used to power peripherals.

Note, the Delay 0 functionality of the CR300/CR350 is different than other Campbell Scientific dataloggers, which only keep the excitation channel on until the end of the program scan when Delay is 0. In other words, other Campbell Scientific dataloggers do not use ExciteV to provide continuous power to peripheral devices.

The Delay parameter is used to specify the length of time the datalogger will wait before moving on to the next instruction after the excitation is applied. Excitation is turned off after the specified delay. If the Delay is set to 0 (default), the datalogger will apply the desired excitation and immediately move to the next instruction without turning excitation off, as described above.

## Parameters

# ExChan (Excitation Channel)

Specifies theexcitationterminal to use to excite the first measurement. If the Reps parameter is greater than 1, the excitation terminal used for subsequent measurements will be incremented with each measurement based on the MeasPEx parameter.

When a series of sensors are measured with the same instruction, the Meas/Ex parameter is used to determine how many sensors to excite with one excitation terminal before advancing to the next.

An alphanumeric code is entered. Right-click within the parameter to display a list.

| Alphanumeric | Description          |
| ------------ | -------------------- |
| VX1          | Excitation channel 1 |
| VX2          | Excitation channel 2 |

Type: Constant

# ExmV (Excitation Voltage in mV)

The excitation voltage in millivolts. The allowable range is +150 to +2500 mV for all bridge measurements. For the ExciteV() instruction, the range is +150 to +5000 when powered via an external battery (when powered via USB, it is +150 to +2500 mV).

Type: Constant. For ExciteV() only, this parameter can also be a Variable.

# Delay

The amount of time, in microseconds, to delay before moving on to the next instruction. Excitation is turned off after the specified delay. If the Delay is set to 0 (default), the datalogger will apply the desired excitation and immediately move to the next instruction without turning excitation off. The excitation will be held until the end of the program scan or until another instruction sets an excitation.

Type: Constant (or expression that evaluates as a constant)
