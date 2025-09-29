# STOffset (Self-Timed Transmission Offset)

The time after midnight for the first self-timed transmission. The value is a constant string entered in the format of "Hours_Minutes_Seconds". Typically, only the Hours_Minutes parameters are used and Seconds is left at 0, unless the window of transmission is less than 60 seconds. Maximum offset is 23:59:59. This parameter can be set to 0 for no offset.
