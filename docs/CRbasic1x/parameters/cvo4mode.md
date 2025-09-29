# CVO4Mode

Determines what type of signal will be output by the device. Right-click the parameter to display a list box of valid options.

| Option | Description                                      |
| ------ | ------------------------------------------------ |
| 0      | Voltage output, use jumper settings (scale only) |
| 1      | Current output; use jumper settings (scale only) |
| 10     | Voltage output; override jumper setting          |
| 11     | Current output; override jumper setting          |

The two override options affect all of the channels of all of the SDM-CVO4 devices being controlled by this instruction. These two options override the hardware settings in the device. Use of this mode takes approximately 2 milliseconds per device. When either of these options is used you lose the flexibility of setting the output for each channel individually.

Type: Variable
