# DestTherm (Thermistor Values Destination)

The variable array in which to store the thermistor values streaming from the device. The array should be dimensioned to two for the CDM-VW300 or eight for the VWIRE 305 or CDM-VW305. If the SteinA/B/C are non-zero in CDM_VW300Config this value will be in DegC; otherwise, when using zeroes for ABC this value is in Ohms.

Type: Variable array
