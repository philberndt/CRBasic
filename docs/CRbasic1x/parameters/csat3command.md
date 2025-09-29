# Command (CSAT3 Measurement Trigger)

Commands 90 - 92 send a measurement trigger to the CSAT3 with theSDM

**Note:** Synchronous Device for Measurement. A processor-based peripheral device or sensor that communicates with the datalogger via hardwire over a short distance using a protocol proprietary to Campbell Scientific.

address specified by the SDMAddress argument. The CSAT3 also sends data to the datalogger. Options 97 - 99 get data after a group trigger, SDMTrigger(), from the CSAT3 specified by the SDMAddress parameter without triggering a new CSAT3 measurements. The CSAT() instruction must be preceded by the SDMTrigger() instruction in order to use Options 97 - 99. Right-click the parameter to display a list.

| Code | Description                                                         |
| ---- | ------------------------------------------------------------------- |
| 90   | Trigger and get wind & speed of sound data.                         |
| 91   | Trigger and get wind & sonic temperature data.                      |
| 92   | Trigger and get wind & speed of sound data minus 340 m/s.           |
| 97   | Get wind & speed of sound data minus 340 m/s after a group trigger. |
| 98   | Get wind & sonic temperature data after a group trigger.            |
| 99   | Get wind & speed of sound data after a group trigger.               |

Type: Constant
