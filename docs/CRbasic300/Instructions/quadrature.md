# Quadrature

The Quadrature instruction is used to measure the output from an incremental encoder.

## Syntax

Quadrature(Dest,Port,Option,Mult)

The following example program shows the use for the Quadrature instruction. It was written for a CR300 datalogger, but can be used in other dataloggers with slight changes to port number.

```
Public Quad(4), LevelFt
Alias Quad(1) = Accum
Alias Quad(2) = NetDir
Alias Quad(3) = CountsUp
Alias Quad(4) = CountsDown

'multiplier = wheel circumference / counts per rev
'1 ft circumference wheel; 100 counts per revolution
'multiplier = 1/100 = .01
Const Mult = 0.01

'Constant Table to Enter Observed Stage
ConstTable (Observation,0)
Const ObsLevelFt = 0
EndConstTable

BeginProg
LevelFt=ObsLevelFt
Scan (1,Sec,1,0)

Quadrature(Quad(1),C1,0,Mult)
If NetDir <> 0 Then
LevelFt = LevelFt + Accum
EndIf

NextScan
EndProg
```

## Remarks

The Quadrature instruction is used with single ended type (A and B) quadrature encoders. A digital port pair is used to monitor the two control tracks of the encoder. The instruction calculates the displacement and direction of the encoder.

## Parameters

# Dest (Destination)

The variable array in which to store the results of the measurement. Dest must be dimensioned to four to hold the following values:

- **Dest(1)**: Accumulator. Net number of counts from channel A and channel B. Count will increase if Channel A leads Channel B. Count will decrease if Channel B leads Channel A.

- ** Dest(2)**: Net direction. Indicates direction if a change occurs.
  - 1 = A change in pulse count has occurred, where A is leading B since the last stop in pulse count.
  - 0 = No change (default).
  - -1 = A change in pulse count has occurred, where B is leading A since the last stop in pulse count.

- ** Dest(3)**: Number of counts in direction 1 (A leading B) that have occurred since the last time the instruction was executed.

- ** Dest(4)**: Number of counts in direction 2 (B leading A) that have occurred since the last time the instruction was executed.

Type: Variable Array

# Port

Specifies which digital port pair to use for monitoring sensor channels A and B, respectively. Valid ports are:

| Code | Description                   |
| ---- | ----------------------------- |
| C1   | Control ports 1 and 2         |
| SE1  | Single-ended channels 1 and 2 |

Type: Constant

# Option

Determines how each channel is counted.

| Code | Description                                                                                                                                                                                                      |
| ---- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 0    | X1 - Counts will increase on rising edge of Channel A when Channel A leads Channel B. Counts will decrease on falling edge of Channel A when Channel B leads Channel A.                                          |
| 1    | X2 - Counts will increase at each rising and falling edge of Channel A when Channel A leads Channel B. Counts will decrease at each rising and falling edge of Channel A when Channel A leads Channel B.         |
| 2    | X4 - Counts will increase at each rising and falling edge of both channels when Channel A leads Channel B. Counts will decrease at each rising and falling edge of both channels when Channel B leads Channel A. |

Type: Constant

# Mult (Multiplier)

Constant, variable, array, or expression by which to scale the results of the measurement.
