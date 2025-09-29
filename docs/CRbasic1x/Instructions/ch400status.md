# CH400Status (Create CH400 Status Table)

This instruction creates a CH400 Status table in the datalogger that displays diagnostic, charging, and load parameters available from a CH400 battery charger.

## Syntax

CH400Status(Interval,Units)

### Example #1

This program queries the CH400 once per second and creates a CH400Status table that may be viewed with software.

```
'Declare Variables
Public count

CH400Status(1000,msec)

'Main Program
BeginProg

Scan (1000,mSec,0,0)
count = count + 1
NextScan

EndProg
```

### Example #2

This program queries the CH400 once per second, creates a CH400Status table, and creates a final storage table using the elements from the CH400Status table.

```
'Declare Variables
Public count

CH400Status(1000,msec)

DataTable (CH400Table,True,-1 )
Sample (30,CH400Status,IEEE4)
EndTable

'Main Program
BeginProg

Scan (1000,mSec,0,0)
count = count + 1
CallTable CH400Table
NextScan

EndProg
```

## Remarks

When a CH400 battery charger is connected to the datalogger CS /IO port, all diagnostic, charging, and load parameters available from the CH400 are displayed in a CH400 Status table. In order to populate the table, a CH400 must be connected to the datalogger via CS I/O. The CH400 is queried for new information on the interval set in the instruction. Specific fields in the CH400Status table may be accessed using Tablename.Fieldname syntax (for example, CH400Status.ChargeA_Voltage, CH400Status.BattA_Current, etc.). The instruction can be placed before or after the BeginProg, but it cannot be in a scan. The instruction should only be declared once.

## Parameters

# Interval

Enter the time interval on which the CH400 is queried for new information. The unit for the interval is selected with the Units parameter. The interval has to be at least one second and less than or equal to a day (86400 seconds).

Type: Constant (or expression that evaluates as a constant)

# Units

The units for the interval parameter. Right-click the parameter to display a list.

| Code | Description  |
| ---- | ------------ |
| μsec | microseconds |
| msec | milliseconds |
| sec  | seconds      |
| min  | minutes      |
| hr   | hours        |
| day  | day          |

Type: Constant
