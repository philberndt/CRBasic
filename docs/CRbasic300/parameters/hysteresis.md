# Hysteresis

The Hysteresis determines the minimum change in the input that must occur before a crossing is counted. If this value is too small, noise could be counted as crossings. For example, suppose 5 is a crossing level. If the input is not really changing but is varying from 4.999 to 5.001, a Hysteresis of 0 would allow all these crossings to be counted. Setting the Hysteresis to 0.1 would prevent this noise from causing counts.

Type: Constant (or expression that evaluates as a constant)
