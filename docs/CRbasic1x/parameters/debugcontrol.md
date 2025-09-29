# Control

A variable declared as a long that controls whether or not the program breaks as DebugBreak points, and when and how it will resume after the break. The Control codes are:

| Code | Description                                                                            |
| ---- | -------------------------------------------------------------------------------------- |
| 0    | Program execution will proceed normally; no breaks                                     |
| -1   | Program execution will proceed normally until a break point is encountered             |
| 1    | Program execution will break immediately when Control is set to 1                      |
| 2    | Program execution will step to the next instruction                                    |
| 3    | Program execution will step to the next instruction, but over a Function or Subroutine |

Type: Variable declared as Long
