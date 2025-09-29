# GNSSTime (GNSS Time Array)

The variable array in which to store the time in Coordinated Universal Time (UTC) returned by the CellGNSS instruction. This array returns 9 values and must be dimensioned to 9. If no values are available, 0 will be returned. The values in this array are updated each time a fix location is recorded. In the event of a timeout, the fix parameter will be set to 0 and these values will not be updated.

The following values are returned by the CellGNSS instruction:

- **Array(1)**: Year

- ** Array(2)**: Month

- ** Array(3)**: Day

- ** Array(4)**: Hours

- ** Array(5)**: Minutes

- ** Array(6)**: Seconds

- ** Array(7)**: Microseconds

- ** Array(8)**: Day of week (Sunday = 1; (1..7)

- ** Array(9)**: Day of year (1..366)

** NOTE:**Because the CELL230 does not report microseconds, day of week, or day of year, 0 is returned for array elements 7 to 9.

Type: Variable
