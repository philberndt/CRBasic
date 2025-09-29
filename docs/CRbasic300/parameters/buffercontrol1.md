# BufferControl (Buffer to Use)

BufferControl is aBoolean

**Note:** Data type used to represent conditions or hardware that have only two states (true or false) such as flags and control ports.

parameter (0 or non-zero) that specifies which buffer in the transmitter to write to. The parameter may be a constant or a variable expression. If the parameter is a constant, then 0 specifies a self-timed buffer and 1 or non-zero specifies write to the random buffer. If outputting to the self-timed buffer, output is controlled by whether there is a new record or not. To conditionally output to the random buffer, specify the Buffer Control parameter as a variable expression. When the expression evaluates as True (non-zero), output is written to the random buffer. False (0) means do not output.

**NOTE:** The Self-Timed Interval must be at least 5 minutes greater than the Random Interval. Setting the Self-Timed Interval smaller than the Random Interval is atypical, but if done will result in a timing conflict between the two tasks.

| Parameter Type      | Value           | Function                                          |
| ------------------- | --------------- | ------------------------------------------------- |
| Constant            | 0               | Write to self-timed buffer when new record occurs |
| Constant            | 1 (non-zero)    | Write to random buffer when new record occurs     |
| Variable Expression | True (non-zero) | Output right now to random buffer                 |
| Variable Expression | False (0)       | No output                                         |

Type: Constant (always write to either buffer) or Variable as Boolean (conditionally output to random buffer)
