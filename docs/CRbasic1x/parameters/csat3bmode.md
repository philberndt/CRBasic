# Mode (Trigger Source and Filter)

Defines the trigger source and application of filters to the anemometer data.

| Code | Description                                                      |
| ---- | ---------------------------------------------------------------- |
| 0    | Datalogger triggered/no filter/datalogger prompted output        |
| 5    | Self triggered/5 Hz bandwidth filter/datalogger prompted output  |
| 10   | Self triggered/10 Hz bandwidth filter/datalogger prompted output |
| 25   | Self triggered/25 Hz bandwidth filter/datalogger prompted output |

**NOTE:** With the SDM Bus, only Mode 0 is supported. All other options require the CPI bus.

Type: Constant
