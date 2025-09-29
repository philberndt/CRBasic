# GNSSArray (GNSS Array)

The variable array in which to store the information returned by the CellGNSS instruction. This array returns 8 values and must be dimensioned to 8. If no values are available, 0 will be returned. The values in this array are updated each time a fix location is recorded. In the event of a timeout, the fix parameter will be set to 0 and these values will not be updated.

The following values are returned by the CellGNSS instruction:

- **Array(1)**: Fix quality (0 = no fix - timeout; 2 = 2D fix; 3 = 3D fix)

- ** Array(2)**: Latitude (-)dd.ddddd,(-)ddd.ddddd

- ** Array(3)**: Longitude (-)dd.ddddd,(-)ddd.ddddd

- ** Array(4)**: Elevation, meters

- ** Array(5)**: Speed over ground, Km/h (SPKM)

- ** Array(6)**: Course over ground, degrees (COG)

- ** Array(7)**: Horizontal precision (HDOP): 0.5-99.9

- ** Array(8)**: Number of satellites

Type: Variable
