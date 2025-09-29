# Units

The Units parameter specifies the units on which the TintoInt and Interval arguments will be based. The options are msec (milliseconds), sec (seconds), min (minutes), hr (hours), day (days), or mon (month).

When days are used, TintoInt starts on a Monday (since January 1, 1990 fell on a Monday). For example, TimeIntoInterval(0,7,days) would perform a function every Monday. TimeIntoInterval(1,7,days) would perform a function every Tuesday, and so on.

When month (mon) is used for the Units parameter, the Interval parameter is specified as an integer ranging from 0 (January) to 11 (December), and the TIntoInt parameter is specified as seconds into the month. The month Interval is synchronized to the beginning of the year (January at 00:00 hours). When the dataloggers real-time clock reaches 00:00 on the last day of the specified month, the month increments by 1, at which point the month interval condition is True. For example, consider the case where the month interval is specified as 9 (October). When the dataloggers real-time clock reaches 00:00 on October 1st, month changes to 10. 10 mod 9 = 1, so the True condition is met.

The TintoInt parameter with mon chosen as the units denotes either seconds before the next month, or seconds into the next month, depending on whether a negative or positive value is entered. For instance, TimeIntoInterval(-60, 9, Mon) is true for one second, one minute before October 1st. Conversely, TimeIntoInterval(60, 9, Mon) is true for one second, one minute after October 1st begins. Note that TintoInt values must align with your scan interval. If your scan interval is one minute, TintoInt should be set to 60 seconds or a multiple thereof to trigger a true condition.

**NOTE:** When using month as the Interval unit, any month other than January results in the True condition occurring twice a year; once at the start of January and once at the end of the specified month interval.

Type: Constant
