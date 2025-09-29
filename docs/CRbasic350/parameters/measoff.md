# MeasOff (Measure Offset Voltage)

Determines whether the ground offset voltage is measured before the measurement is made on the analog channel. If the ground offset is measured it is subtracted from the sensor measurement and the result is stored in Dest.

| Value | Result                                                                                                                                               |
| ----- | ---------------------------------------------------------------------------------------------------------------------------------------------------- |
| 0     | Offset voltage is corrected from background calibration.                                                                                             |
| 1     | Offset voltage is measured each scan (this option effectively increases the measurement time as if an additional rep were added to the instruction). |

Type: Constant
