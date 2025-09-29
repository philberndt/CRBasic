# RunReset

When RunReset is true, the history over which the running value (for example, running average, running maximum, running minimum, running total, or running standard deviation) is calculated is cleared, so that the value returned is calculated from a a single value, the current input. Note that the reset parameter does not automatically toggle back to false once set to true. When the reset parameter is set back to false (problematically or manually), then the running average, running maximum, running minimum, or running total continues, starting with the current input. Note that until the data logger has taken N measurements, the running average/total/maximum/minimum is calculated based on the actual number of measurements made since reset was set back to false.

Type:Boolean

**Note:** Data type used to represent conditions or hardware that have only two states (true or false) such as flags and control ports.

variable or expression that evaluates true/false
