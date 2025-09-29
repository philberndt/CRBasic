# NoiseRejectionFreq

The type of noise rejection to be applied to the measurement. Valid options are:

| Code | Description                                                                                                                                                                                                                       |
| ---- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 0    | No frequency rejection will be used in the measurement.                                                                                                                                                                           |
| 50   | The TDR will take 2 contiguous measurements that are exactly 10 ms out of phase in order to cancel 50 Hz noise. If the WaveAvg parameter is an odd number, the actual number of averages will be the next highest even integer.   |
| 60   | The TDR will take 2 contiguous measurements that are exactly 8.33 ms out of phase in order to cancel 60 Hz noise. If the WaveAvg parameter is an odd number, the actual number of averages will be the next highest even integer. |

Type: Constant
