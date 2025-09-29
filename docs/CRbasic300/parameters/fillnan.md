# FillNAN (Fill NAN)

An optional parameter that indicates how NAN values due to bad sensor readings are recorded. The default behavior if this parameter is not present is that NAN is written only to the first element in the array and the remaining elements of the array contain the last known good values. The options for this parameter are:

- **0 **: Default behavior; write NAN only to the first element in the array

- **-1 **: Fill the entire array with NAN

- **>0**: Fill the specified number of elements in the array with NAN, beginning with the starting element defined by Dest

Type: Constant
