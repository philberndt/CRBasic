# TimeOut (Wait Time)

For the SerialIn instruction, the TimeOut parameter is used to specify the amount of time, in 0.01 seconds, that the datalogger should wait before proceeding to the next instruction. If a 0 is entered for this parameter, the datalogger will wait for the termination character (TerminationChar parameter) or maximum number of characters (MaxNumChars parameter) to be received before proceeding. TimeOut is reset with each new incoming character.

For the SerialOut instruction, the TimeOut parameter is used to specify the amount of time, in 0.01 seconds, that the datalogger should wait for the WaitString or echo of each character in the OutString. If the TimeOut is 0, the datalogger does not wait (or even check) for the WaitString or Echo before proceeding to the next instruction. The TimeOut applies to each of the NumberTries; the overall timeout period for the instruction is cumulative. TimeOut is reset with each new incoming character.

As a result of internal buffering in the datalogger and/or external interfaces, data may not appear in the serial port buffer for a period of up to 50 ms (depending on the serial port being used). If the TimeOut is set at too short of an interval, the instruction may time out even though data has been received in the internal buffer (though not yet passed on to the serial port buffer). This should be taken into consideration when setting the TimeOut parameter.

Type: Constant
