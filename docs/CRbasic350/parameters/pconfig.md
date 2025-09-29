# PConfig (Pulse Channel Configuration)

A code specifying how the pulse channel should be configured. Right-click the parameter to display a list.

**NOTE:** Code 0 may be used to disable the measurement during cleaning or calibration, if the PConfig parameter is a variable.

| Code | Description                                                                                                                                   |
| ---- | --------------------------------------------------------------------------------------------------------------------------------------------- |
| 0    | High frequency; all channels                                                                                                                  |
| 1    | Low level A/C: P_LL only                                                                                                                      |
| 2    | Switch closure; C1, C2, P_SW, SE1, SE2, SE3, SE4 **NOTE:** Switch closure on C1, C2, SE1, SE2, SE3, SE4requires a 100 kΩ 12V pull-up resistor |

Type: Constant (or Variable that evaluates to 0, 1,or 2)
