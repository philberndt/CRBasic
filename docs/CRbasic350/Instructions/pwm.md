# PWM (Pulse Width Modulation)

The PWM instruction is used to provide pulse width modulation control usingone of four SE channels.

## Syntax

PWM(Source,Port,Period,Units)

This example program shows simple PID control and use of the PWM instruction. In this example the pressure of a manifold is controlled by driving a proportional solenoid in the gas stream using high speed pulse width modulation.

Note that the PID gain terms are very application specific and need to be determined either from theory and/or experimentationCampbell Scientificcannot provide support on this process. As a general rule is it best to turn on the P, I and D terms in that order and establish suitable gain parameters for each term before enabling the next. Some control systems do not require all terms.

```
Const ScanRate_ms = 1000 'ScanRate in milliseconds

'Pressure Control Variables:
Public Pressureread
Public UseP As Boolean, UseI As Boolean, UseD As Boolean' Set to Run ControlAlgorithm
Public PressureSetPt
Public P, I, D
Public P_output, I_output, D_output
Public DutyCycle
Public Pfact, Ifact, Dfact, MaxI
Public PrevPress, IntegrateON As Boolean
'________________________________________

BeginProg
' Pressure Control initialization:
' Declared as public variables as it allows you to tweak
' the control terms whilerunning
DutyCycle = .5
Pfact = 0.0025
Ifact = .000125
Dfact = .001
MaxI = 100000

UseP = true
UseI = true
UseD = true
IntegrateOn = true

Pressuresetpt=10
'_____________________________
Scan (ScanRate_ms,mSec,10,0)

'....Other measurements

'Measure the pressure. Would normally have appropriate MX and offset
VoltDiff (Pressureread,1,mv2500,1,True ,0,4000,1,0)

'PRESSURE CONTROL
'Proportional term, i.e. the current error from the setpoint
P = Pressureread - PressureSetPt
'Integral term
If P<> NAN AND IntegrateON Then I = I + P
If I > MaxI Then I = MaxI
If I < -MaxI Then I = -MaxI
'Differential term
D = Pressureread - PrevPress
'Apply the gain factors to each term
P_output = P * PFact
I_output = I * Ifact
D_output = D * Dfact
'Update the previous reading
PrevPress = Pressureread
'If any of the terms are in use reset the duty cyle
If UseP OR UseI OR UseD Then
DutyCycle = 0
EndIf
'The add each term to form the new duty cycle.
If UseP Then DutyCycle = DutyCycle + P_output
If UseI Then DutyCycle = DutyCycle + I_output
If UseD Then DutyCycle = DutyCycle + D_output
If DutyCycle > 1 Then DutyCycle = 1
If DutyCycle < 0 Then DutyCycle = 0

'Call the PWM instruction to control the opening of the valve.
'PWM(Src,channel,period,units)
' Src cannot be long or string. it is the duty cycle input
' Duty is fraction from 0.0 -> 1.0
' period, and units must be constants (known at compile time)
PWM(DutyCycle,4, 200, uSec)

NextScan
EndProg
```

## Remarks

The PWM instruction programs the datalogger hardware based on the duty cycle (Source) and period. Its operation is independent of the datalogger scan interval. The duty cycle will remain set until the PWM instruction changes it.

## Parameters

# Source

Used to specify the duty cycle for the instruction. It is a constant or variable specified as a value of 0.0 <= value <= 1.0, where 0.0 is always off (port low) and 1.0 is always on (port high).

Type: Constant or Variable (cannot be declared as string or long)

# Port

The digital channel to be used for this instruction. Valid options areSE1, SE2, SE3,SE4, and C1.Right-click the parameter to display a list of valid options.

Type: Constant

# Period

A constant or variable that is used to specify the period for the signal. Maximum period is 34952 milliseconds.

Resolution for different periods:

- 0 <= period <= 5.4 mS, resolution is 83.33 nS

- 5.4 mS < period <= 21 mS, resolution is 333.33 nS

- 21 mS < period <= 131 mS, resolution is 2.0 uS

- 131 mS < period <= 1092 mS, resolution is 16.66 us

- 1092 mS < period <= 10922 mS, resolution is 166.66 uS

- 10922 mS < period <= 34952 mS, resolution is 533.333 uS

Type: Constant or variable

# Units

The unit for the Period parameter. Valid units are microseconds (usec), milliseconds (msec), or seconds (sec). Right-click the parameter for a list of valid options.

Type: Constant
