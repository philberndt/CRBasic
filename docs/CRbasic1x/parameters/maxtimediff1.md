# MaxTimeDiff (Maximimum Difference in Time)

The maximum difference in time (ms) between the datalogger clock and theDNP3

**Note:** Distributed Network Protocol is a set of communications protocols used between components in process automation systems. Its main use is in utilities such as electric and water companies.

master clock that will be tolerated before the clock is changed upon command from the DNP3 master. If a 0 is entered, the datalogger clock will be set upon receipt of a time synchronization command from the DNP3 master. If a -1 is entered, the datalogger clock will not be set. If the datalogger is connected to a DNP3 master and a Loggernet server, a -1 can be entered in order to avoid problems that can arise from a DNP3 master and Loggernet server both setting the datalogger clock.

Type: Constant or Variable
