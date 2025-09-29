# TimeOffset (Time Offset)

The local time offset, in seconds, from UTC. The TimeOffset parameter is ignored if the "UTC Offset" setting in the datalogger is a value other than -1 (disabled). In other words, the UTCOffset setting in the datalogger overrides this instruction parameter. The UTCOffset setting can be found in the Settings table. For more information, seeSettings Available Using SetSetting

**NOTE:** For the GPS() instruction, in order to use GPS coordinates without setting the datalogger clock, set the TimeOffset parameter to -1.

Type: Constant
