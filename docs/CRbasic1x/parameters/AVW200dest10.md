# Destination (Dest)

The variable array in which to store the results of the instruction. Dest is a single-dimensioned array of 5 or 6 (depending upon whether a thermistor is being measured) if only one sensor is being measured. Dest is a multi-dimensioned array of 5 or 6 if multiple sensors are being measured using a multiplexer. The first dimension is set equal to the number of sensors being measured and the second dimension is set equal to the number of values returned for each sensor (5 or 6). For example, to measure 4 sensors with thermistor measurements attached to a multiplexer, Dest would be declared as Array(4,6). Values for sensor 1 would be stored in Array(1,1) through Array(1,6), values for sensor 2 stored in Array (2,1) through (2,6), etc.

Type: Variable array
