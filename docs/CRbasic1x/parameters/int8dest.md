# Dest

The variable or array where the results of the instruction are stored. For all output options except Capture All Events, the Dest argument should be a one-dimensional array with as many arguments as there are programmed INT8 channels. If the Capture All Events output option is selected, then the Dest array must be two dimensional. The magnitude of the first dimension should be set to the number of functions (up to 8), and the magnitude of the second dimension should be set to at least the maximum number of events to be captured. The values will be loaded into the array in the sequence of all of the time ordered events captured from the lowest programmed channel to those of the highest programmed channel.

Type: Variable Array
