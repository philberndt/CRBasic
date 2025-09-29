# Function

Specifies the type of calibration to be performed. Right click the parameter to display a list:

| Code | Calibration Type                                                                                                                                                                                                                                                               |
| ---- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| 0    | Zeroing Calibration. The offset value is adjusted so that the measurement result is exactly zero while the sensor is in the calibrated state (known zero condition). The multiplier is not adjusted.                                                                           |
| 1    | Offset Calibration. The offset value is adjusted so that the measurement result gives a specified, desired value while the sensor is in its calibrated state (known condition). The multiplier is not adjusted.                                                                |
| 2    | Two Point, Multiplier and Offset. The multiplier and offset values are adjusted to make a linear fit between the two calibration input states (two known conditions).                                                                                                          |
| 3    | Two Point, Multiplier Only. The multiplier value is adjusted to make a best fit between the two calibration input states (two known conditions). The offset is not adjusted.                                                                                                   |
| 4    | Zero Basis Point. A measurement value (usually non-zero) is stored for later reference, comparison, or calculation purposes (Baseline or Datum point). This value corresponds to a zero condition or baseline sensor state. Neither the multiplier nor the offset is adjusted. |

Type: Integer
