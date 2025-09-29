# Multipliers, Offsets, and Disable Variables with Repetitions

Most measurement instructions have a Repetitions (Reps) parameter, that allows you to measure multiple sensors with one instruction if the sensors are wired into consecutive terminals. Most measurement instructions also use a Multiplier and an Offset parameter that can be used to scale the resulting measurement or convert it to a different engineering unit. Repetitions can also be used with the Multiplier and Offset parameters to apply a different multiplier and/or offset to each repetition. This can be helpful in measuring multiple sensors that, while they are the same sensor, may have different calibration values.

This same syntax can be used for the DisableVar parameter for most output instructions.

As an example, consider the following program:

```
Public Temp(5), Mult(5), PTemp

BeginProg
Mult(1)=1
Mult(2)=10
Mult(3)=100
Mult(4)=1000
Mult(5)=10000
Scan (1,Sec,3,0)
PanelTemp (PTemp,4000250)
TCDiff (Temp(),5,mv34mV2_5,11,TypeT,PTemp,True,0,4000250,Mult(),0)
NextScan
EndProg
```

The [TCDiff](../Instructions/tcdiff.md) instruction is programmed for five repetitions. With the first execution of the instruction during a program scan, a Multiplier of 1 is used. With the second execution, a multiplier of 10 is used. With the third execution, a multiplier of 100 is used, and so on.

The syntax used for the Multiplier and/or Offset must be considered carefully, or the results may not be as you expect. Consider the following example for a multiplier variable (Mult) and the results based on syntax:

| Variable used | Result                                                                                                       |
| ------------- | ------------------------------------------------------------------------------------------------------------ |
| Mult          | Instruction uses the first variable, Mult(1), for all repetitions                                            |
| Mult( )       | Instruction starts with the first variable, Mult(1), and the Multiplier is incremented with each repetition  |
| Mult(1)       | Instruction uses the first variable, Mult(1), for all repetitions                                            |
| Mult(2)       | Instruction uses the second variable, Mult(2), for all repetitions                                           |
| Mult(2)( )    | Instruction starts with the second variable, Mult(2), and the Multiplier is incremented with each repetition |

Note that if an expression is used for the Multiplier or Offset (for example, Mult(1) \* 3), all reps will use the same value, regardless of the syntax used.
