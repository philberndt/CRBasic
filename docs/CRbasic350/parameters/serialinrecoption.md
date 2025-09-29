# SerialInRecOption (Serial In Record)

Determines what record from the serial buffer is stored into Dest and whether the datalogger loads aNAN

**Note:** Not a number. A data word indicating a measurement or processing error. Voltage overrange, SDI-12 sensor error, and undefined mathematical results can produce NAN.

value or if the last value loaded will remain. Option codes include:

| Code | Description                                                         |
| ---- | ------------------------------------------------------------------- |
| 00   | Most recent record in serial buffer, do not store NAN if no records |
| 01   | Most recent record in serial buffer, store NAN if no record         |
| 10   | Oldest record in serial buffer, do not store NAN if no record       |
| 11   | Oldest record in serial buffer, store NAN if no record              |

Add 100 to the Code to use a local read pointer (i.e., 100, 101, 110, 111).

If option 01 or 11 is chosen and no new record is received, the NAN value stored in Dest will depend upon its data type. If Dest is formatted as a float, NAN will be stored. If it is formatted as a LONG, &H80000000 will be stored. If Dest is formatted as a string, a NAN will be stored.
