# WaitString (Wait String)

An incoming string for which the datalogger should wait before proceeding to the next instruction after the OutString has been sent. The WaitString will be formatted as a string by the datalogger if it is not already in that format.

A null ("") WaitString can be entered to tell the datalogger to wait for the echo of each character in the OutString. The datalogger sends the OutString one character at a time and looks for the echo of each character. If the TimeOut is 0, the datalogger does not wait for the WaitString or echo of the OutString. The datalogger will simply send each character of the OutString the NumberTries and move on to the next instruction. If TimeOut > 0, the datalogger will wait for an echo of each character sent in the OutString, failing only when the TimeOut and NumberTries are met.

Type: Variable
