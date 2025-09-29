# PConfig (Pulse Channel Configuration)

A code specifying how the pulse channel should be configured. Right-click the parameter to display a list.

**NOTE:** Code 0 may be used to disable the measurement during cleaning or calibration, if the PConfig parameter is a variable.

| Code | Description                                       |
| ---- | ------------------------------------------------- |
| 0    | Disabled                                          |
| 1    | Switch Closure with pull up                       |
| 2    | Switch Closure with pull down(Control ports only) |
| 3    | High frequency with pull up                       |
| 4    | High frequency with pull down(Control ports only) |
| 5    | Low Level AC(Pulse channels only)                 |

Type: Constant (or Variable that evaluates to 0, 1,2, 3, 4, or 5)
