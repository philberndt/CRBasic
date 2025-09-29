# Dest (Destination)

The variable array in which the results of the instruction will be written. Dest must be dimensioned to 5 or a compilation error will occur. The following 5 values are returned:

- Dest(1) = SolarAzimuth (degrees from North; N = 0, E = 90, S = 180, W = 270)

- Dest(2) = SunElevation (degrees above the horizon; 90 degrees corresponds to solar noon)

- Dest(3) = HourAngle (Radians West from solar noon). Converts local solar time into an angular displacement for the sun's movement across the sky.

- Dest(4) =Declination (Radians North or South). Angle between the equator and the location of the sun.

- Dest(5) = AirMass (air mass coefficient). Ratio of the path length of the current solar position to the path length at solar noon. Calculated via Kasten and Young s method 1989. If a pressure measurement is possible, a correction will be applied and will refer to the airmass as an absolute airmass. If pressure is not used, airmass may be referred to as a relative airmass.
