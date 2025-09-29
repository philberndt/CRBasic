# Funct4_1

A four digit code used to program the timing function of channels 1 through 4. Similar to the Config parameters, digits represent the channels in descending order from left to right (e.g., 4 3 2 1).

| Code | Description                                                                             |
| ---- | --------------------------------------------------------------------------------------- |
| 0    | No value returned                                                                       |
| 1    | Period (ms) between edges on the programmed channel                                     |
| 2    | Frequency (kHz) of edges on the programmed channel                                      |
| 3    | Time (ms) between an edge of the previous channel and an edge of the programmed channel |
| 4    | Time (ms) between an edge on Channel 1 and edge on the programmed channel               |
| 5    | Number of edges on channel 2 since last edge on channel 1 using linear interpolation    |
| 6    | Low resolution frequency (kHz) of edges on programmed channel                           |
| 7    | Total count of edges on programmed channel since last interrogation                     |
| 8    | Number of edges on channel 2 since last edge on channel 1 without linear interpolation  |

Type: Constant
