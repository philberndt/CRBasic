# Option

Determines how each channel is counted.

| Code | Description                                                                                                                                                                                                      |
| ---- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 0    | X1 - Counts will increase on rising edge of Channel A when Channel A leads Channel B. Counts will decrease on falling edge of Channel A when Channel B leads Channel A.                                          |
| 1    | X2 - Counts will increase at each rising and falling edge of Channel A when Channel A leads Channel B. Counts will decrease at each rising and falling edge of Channel A when Channel A leads Channel B.         |
| 2    | X4 - Counts will increase at each rising and falling edge of both channels when Channel A leads Channel B. Counts will decrease at each rising and falling edge of both channels when Channel B leads Channel A. |

Type: Constant
