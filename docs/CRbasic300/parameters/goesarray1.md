# GOESArray1 (GOES Array Result 1)

An array that holds a result code indicating the success of the instruction, followed by global positioning information.

The result codes are as follows:

| Code | Description                                                                     |
| ---- | ------------------------------------------------------------------------------- |
| 0    | Command executed successfully.                                                  |
| 2    | Timed out waiting for STX character from transmitter after SDC addressing.      |
| 3    | Wrong character received after SDC addressing                                   |
| 4    | Something other than ACK returned when select data buffer command was executed. |
| 5    | Timed out waiting for ACK.                                                      |
| 6    | Port not available; GOES not attached.                                          |
| 7    | ACK not returned following data append or overwrite command.                    |

The GPS data values are as follows:

| Value              | Description                                   |
| ------------------ | --------------------------------------------- |
| Time               | Seconds since January 1, 2000                 |
| Latitude           | Fractional degrees; 100 nanodegree resolution |
| Longitude          | Fractional degrees; 100 nanodegree resolution |
| Elevation          | Signed 32-bit number, in centimeters          |
| Magnetic Variation | Fractional degrees; 1 millidegree resolution  |

Type: Variable array dimensioned to 6
