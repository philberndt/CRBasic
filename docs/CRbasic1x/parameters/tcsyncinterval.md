# TCSyncInterval

The interval on which to run the TimedControl sequence. When the program is compiled and starts running or when TimedControl is reset, the program will wait until an even multiple of the SyncInterval to start the sequence. Enter 0 to start immediately.

The ClockOption parameter determines whether the SyncInterval will be restarted or remain unchanged when the datalogger's clock is changed or the instruction is reset.

Type: Constant or Variable declared as Type Long
