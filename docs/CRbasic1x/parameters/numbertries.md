# NumberTries (Number of Tries)

The total number of times the datalogger should attempt to send the OutString. If NumberTries is 0, the OutString is sent only once without checking for a WaitString. The length of the string sent is returned. If NumberTries is 1 the OutString is sent only once but the instruction checks for the WaitString. If the WaitString is received the length of the WaitString is returned.

If the WaitString or echo of the OutString is received before the NumberTries is met, the communication is successful and the datalogger executes the next instruction. If the WaitString is null, the datalogger will wait the NumberTries for an echo of each character output. If a negative NumberTries is entered and the WaitString is null, the datalogger will exit the instruction after the first failure of an echo (where failure = the amount of time specified in the TimeOut parameter has elapsed prior to receiving the echoed character).

Type: Constant, Variable, or Integer
