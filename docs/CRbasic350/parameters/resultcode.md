# ResultCode (Result Code)

The Variable in which to store the results indicating the success of the program instruction. For the GOESData and GOESSetup instructions, this array is dimensioned to 1. For the GOESStatus instruction, the size of the array required will depend upon the option chosen for the StatusCommand. Right-click the parameter to display a list of defined variables.

| Code | Description                                                                     |
| ---- | ------------------------------------------------------------------------------- |
| 0    | Command executed successfully.                                                  |
| 2    | Timed out waiting for STX character from transmitter after SDC addressing.      |
| 3    | Wrong character received after SDC addressing                                   |
| 4    | Something other than ACK returned when select data buffer command was executed. |
| 5    | Timed out waiting for ACK.                                                      |
| 6    | Port not available; GOES not attached.                                          |
| 7    | ACK not returned following data append or overwrite command.                    |
| -11  | Buffer control error.                                                           |
| -12  | Message window error.                                                           |
| -13  | Channel error.                                                                  |
| -14  | Baud error.                                                                     |
| -15  | RCount error.                                                                   |
| -16  | Illegal data format.                                                            |
| -17  | Data format 0 or 1 was chosen, but table values were not FP2 or ASCII.          |
| -18  | STInterval error.                                                               |
| -19  | STOffset error.                                                                 |
| -20  | RInterval error.                                                                |
| -21  | Platform ID error.                                                              |
| -22  | Transmitter not set up.                                                         |

Type: Variable or Array
