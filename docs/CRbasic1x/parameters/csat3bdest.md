# Dest (Destination Variable)

A variable array that stores the values returned by the anemometer. It must be declared as a float with at least 5 elements in the array. The sensor returns the following data in response to a measurement trigger:

- **Dest(1)**: ux, x-axis wind speed in meters per second

- ** Dest(2)**: uy, y-axis wind speed in meters per second

- ** Dest(3)**: uz, z-axis wind speed in meters per second

- ** Dest(4)**: Ts, sonic temperature in degrees C

- ** Dest(5)**: Diagnostic word (refer to the [CSAT3B manual](https://www.campbellsci.com/csat3b) for a table of diagnostic word flags)

Type: Variable array
