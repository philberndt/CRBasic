# BufferControl (Control Buffer)

Used to specify which buffer should be used (random or self-timed) and whether data should be overwritten or appended to the existing data. Data stored in the self-timed buffer is transmitted only during a predetermined time frame. Data is erased from the transmitter's buffer after each transmission.

Data in the random buffer is transmitted immediately after a threshold has been exceeded. The transmission is randomly repeated to insure it is received. Data in the random buffer must be erased using buffer control, code 9, after random transmissions are finished.

A numeric value is entered for this parameter. Right-click to display a list.

| Code | Description                  |
| ---- | ---------------------------- |
| 0    | Append to self-timed buffer. |
| 1    | Overwrite self-timed buffer. |
| 2    | Append to random buffer.     |
| 3    | Overwrite random buffer.     |
| 9    | Clear random buffer          |

The datalogger orders the data from the newest to the oldest. If oldest to newest is needed, execute the instruction in Append mode each time data is written to the data record.

Type: Integer
