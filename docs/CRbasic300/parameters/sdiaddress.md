# SDIAddress (SDI Address)

Enter the SDI12 address that will be affected by this instruction. (For SDI12SensorSetup instruction it is the address to assign to the datalogger. For the SDI12Recorder instruction it is the address of the SDI12 sensor.) Valid range are 0 through 9, A through Z, and a through z. Alphabetical characters should be enclosed in quotes (for example, "A"). The same commands can be sent to multiple sensors by specifying multiple addresses, enclosed in quotes, in this parameter (for example, 012 send the commands to sensors addressed as 0, 1, and 2). The Dest variable should include an extra dimension to accommodate multiple sensors, or, if Dest is declared as a string, all values from each sensor are returned into a separate element of the array (for example, Dest(1) contains all values from sensor 1, Dest(2) contains all values from sensor 2, etc.). .

When sending commands to multiple sensors, all of the C! commands are performed first, and then, while waiting on the C! commands to finish, non-C! commands are performed.

Type: Constant or Variable
