# DisableLithium

DisableLithium is a Boolean setting in the datalogger, that when set to True, puts the datalogger into a power saving mode when external power is removed. This setting is intended to preserve the life of the lithium battery in the datalogger when it is stored for extended periods of time.

When the datalogger is in this "hibernation" mode, the internal clock and RAM are not maintained. Thus, when the datalogger is powered back up, the clock will not be accurate, no program will be running, and data tables from the last running program will no longer be accessible. Also, the DisableLithium setting will be returned to its default of 0.

This setting can be set using Device Configuration Utility.
