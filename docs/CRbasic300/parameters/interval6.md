# Interval

The Interval parameter is used to define how frequently the TimeIsBetween statement will evaluate as True, based on the datalogger's real-time clock. This parameter must be an integer. If a variable is used in this parameter, it is recommended to define it as a Long. Use of non-integers may result in the interval not evaluating as True when expected.

Type: Constant or Variable (declared as Long, recommended)
