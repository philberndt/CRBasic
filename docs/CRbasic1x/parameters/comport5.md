# ComPort (Communication Port)

The ComPort parameter is the control port pair to which the GPS device is attached. Rx is used to read in the NMEA sentences and Tx is used to monitor the PPS from the GPS. COMC1 is the only Comport that supports a PPS signal. Negate the ComPort parameter to program the datalogger to use NMEA sentences for time-syncing rather than the PPS line input (see Remarks above). Valid options are:

- COMC1 C1/C2 (only comport that supports PPS signal)

- -COMC1 to -COMC7 (negated comport for NMEA sentences time-syncing)

- -COMRS232 (negated comport for NMEA sentence time-syncing)

This instruction defaults to a baud rate of 38,400 bps. If the GPS device requires a different baud rate, use the SetStatus instruction in the program to override the default.

Type: Constant
