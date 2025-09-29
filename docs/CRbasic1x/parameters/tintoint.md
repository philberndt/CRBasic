# TintoInt (Time into Interval)

Allows an offset to the specified Interval. The Units for time are the same as for the Interval. This parameter must be an integer. Use of non-integers may result in the interval not evaluating as True when expected. In the DataInterval instruction, this parameter must be a constant (a variable is not allowed). If a variable is used in this parameter, it is recommended to define it as a Long.

TintoInt parameter example usage: if the Interval is set at 60 minutes, and TintoInt is set to 5, then data storage will occur at 5 minutes into the hour, every hour, based on the datalogger's real-time clock. If the TintoInt is set to 0, data storage will occur at the top of the hour.

Type: Constant (required for DataInterval instruction) or Variable (declared as Long, recommended).
