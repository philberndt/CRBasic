# BridgeType

Used to determine the appropriate scaling to perform on the output.

| Code | Bridge Type                                                                                |
| ---- | ------------------------------------------------------------------------------------------ |
| 0    | No bridge. Return the mV measured value.                                                   |
| 1    | Full bridge. Return 1000\*V1/Vx (mv/V) like the BrFull instruction does in the datalogger. |
| 2    | Half bridge. Return V1/Vx (mV/mV) like the BrHalf instruction does in the datalogger.      |
