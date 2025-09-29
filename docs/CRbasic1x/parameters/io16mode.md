# Mode

Used to configure a bank of four ports when a Command code 86 through 90 is used (if any other Command Code is used, enter 0 for the Mode parameters). Mode is entered as a four digit parameter, where each parameter indicates the setting for a port. Ports are represented from the highest port number to the lowest, from left to right (e.g., 16 15 14 13; 12 11 10 9; 8 7 6 5; 4 3 2 1). There is a Mode for Ports 16 - 13, 12 - 9, 8 - 5, and 4 - 1. The valid codes are:

| Code | Description                                                       |
| ---- | ----------------------------------------------------------------- |
| 0    | Output logic low                                                  |
| 1    | Output logic high                                                 |
| 2    | Input digital, no debounce filter                                 |
| 3    | Input switch closure 3.17 msec debounce filter                    |
| 4    | Input digital interrupt enabled, no debounce filter               |
| 5    | Input switch closure interrupt enabled 3.17 msec, debounce filter |
| 6    | Undefined                                                         |
| 7    | Undefined                                                         |
| 8    | Undefined                                                         |
| 9    | No change                                                         |

Type: Constant
