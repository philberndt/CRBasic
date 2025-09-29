# DateOption

Specifies the format of the date/time string used as the source or the format of the date/time string to be returned by the function. Note: The resolution of this function is 1 second. Fractional seconds are not used during conversion from the date/time string to seconds since 1990. If they are included, the timestamp will be truncated. Right-click the parameter to display a list of options:

| Option | Time Stamp Element                                                                                                                                                                              |
| ------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1      | MM/dd/yyyy HH:mm:ss                                                                                                                                                                             |
| 2      | ddd, dd MMM yyyy HH:mm:ss GMT Note: Greenwhich Mean Time if UTC Offset setting is set. If UTC Offset is not set, time returned will be datalogger time and GMT will be omitted from the string. |
| 3      | "dd/MM/yyyy HH:mm:ss"                                                                                                                                                                           |
| 4      | "yyyy-MM-dd HH:mm:ss"                                                                                                                                                                           |
| 5      | "yyyy-MM-dd_HH-mm-ss"                                                                                                                                                                           |
| 6      | "yyyy-MM-dd_HH-mm-ss.ff"                                                                                                                                                                        |

Note: Fractional seconds are always reported as ".00".

Where:

- dd = Day of Month, 01 31

- ddd = day of week (Mon, Tue, Wed, etc)

- ff = Fractional second; always reported as 00

- HH = Hour, 00 - 23

- MM = Month, 01- 12

- MMM = Abbreviated name of the month (Jan, Feb, etc.)

- mm = Minute, 00 - 59

- ss = Seconds, 00 - 59

- yyyy = Year, 0000 - 9999

- GMT = Designation that time displayed is Greenwich Mean Time

Type: Constant
