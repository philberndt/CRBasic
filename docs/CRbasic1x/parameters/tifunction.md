# Function

The Function parameter is a value where each digit represents the function for the channel associated with that digit (see Edges, above). Leading 0s do not have to be used if you are using only the first few channels for the instruction. For example, measure the frequency on the falling edge of C1 could be specified with either: TimerInput(Dest,C1,0,2,0,usec) or TimerInput(Dest,C1,0000,0002,0,usec).

| Function | Result                                                                                                                                                                                                 |
| -------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| 0        | The associated channel is not used.                                                                                                                                                                    |
| 1        | Calculate the period of the signal on the specified channel (in microseconds).                                                                                                                         |
| 2        | Calculate the frequency (Hz) for the specified channel.                                                                                                                                                |
| 3        | Calculate the time from an edge on the previous channel (1 number lower) to an edge on the specified channel (in microseconds)(even channels only, for example,time from C4 since C3, or C8 since C7). |
| 5        | Return the count of edges since the last execution.                                                                                                                                                    |

Type: Constant
