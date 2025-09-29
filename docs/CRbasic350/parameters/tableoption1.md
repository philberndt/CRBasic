# TableOption (Table Option)

Used to specify which records should be sent to the destination PakBus device. This is an optional parameter. Right click the parameter to display a list of valid options:

| Code | Description                                                                                                                                                                                                                                                                                    |
| ---- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 0    | Send the last record stored since the last execution of the instruction. This is the default option if the option code parameter is omitted.                                                                                                                                                   |
| -1   | Send all new records since the last execution of the instruction.                                                                                                                                                                                                                              |
| X    | Send the last X number of records (where X is an integer). If the number of available records is less than X, then all records will be sent. Note that this option can result in sending duplicate records, since it sends a discrete number of records with each execution of the instruction |

Sending multiple records (options -1 or X) increases the time it takes to execute the instruction (and thus, the total time for the datalogger to complete a scan). If skipped scans occur, increase the scan rate of the program or move the SendData to a [SlowSequence](../Instructions/slowsequence.md) scan.

Type: Constant or Variable
