# Reps

Defines the number of sequential SIO4s that will be called by the instruction. The datalogger will poll the SIO4 with the address set by the Address parameter first, receive or send the number of values set by the ValuesPerRep parameter next, and then poll the SIO4 with the next sequential address. If the Reps parameter is 2, the ValuesPerRep is 3, and the Command parameter is set to receive, then three values from the first SIO4 would be sent to the first three elements of the Dest array, and three values from the second SIO4 would be received and written to the fourth through sixth elements of the Dest array.

Constant integer (or expression that evaluates as a constant).
