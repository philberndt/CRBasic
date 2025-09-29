# Settling Time

The settling time is the duration (in microseconds) to allow for signal settling after setting up a measurement (switching to the channel, setting the excitation) and before making the measurement.Minimum settling time is 20 microseconds; maximum settling time is 600 ms. If 0 is entered, a default of 500 microseconds is used.

Additional settling time may be necessary to allow the signal to settle with high resistance or long lead lengths (higher capacitance). The time it will take to make the measurement will include the measurement itself, the SettlingTime, fN1, and whether or not parameters are set to remove voltage offset errors. Using either RevDiff or RevEx causes two SettlingTimes to occur per channel; four SettlingTimes will occur when using both RevDiff and RevEx.

Type: Constant (or expression that evaluates as a constant)
