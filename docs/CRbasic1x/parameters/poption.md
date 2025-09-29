# POption (Pulse Option)

A code that determines if the raw result (multiplier =1, offset = 0) is returned in counts or frequency. Right-click to display a list.

| Code | Description                                                                                                                                               |
| ---- | --------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 0    | Counts                                                                                                                                                    |
| 1    | Frequency in Hz; counts/scan interval in seconds                                                                                                          |
| >1   | Running average of frequency (Hz).The value entered is the time period of the running average in milliseconds and must be a multiple of the scan interval |

Type: Constant
