# SDMAO4ADest

A variable that holds a status code indicating success or failure of the instruction.

| Response Code | Description                          |
| ------------- | ------------------------------------ |
| 240           | Successful                           |
| 241           | Signature error                      |
| 242           | Current overload error               |
| 243           | Current overload and signature error |

A current overload error occurs when current overload protection is triggered (130 mA, +/- 15 mA). A signature error usually indicates noise on the line.

Any other response code returned indicates failed communication.

Type: Variable
