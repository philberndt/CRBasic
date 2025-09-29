# CWSDest (Destination Variable)

A variable array that will hold the sensor values returned by the base. CWSDest should be dimensioned large enough to accommodate all sensor values that will be returned. If CWSDest is not dimensioned large enough to hold all values from a sensor, then no values will be reflected for that sensor and it will appear as if the sensor has not been discovered (i.e., the last few elements of the array will remain at the name of the CWSDestArray rather than changing to reflect the name of the new sensor).

If the base is not properly connected to the datalogger as specified by the CWB100 instruction, a -1 will be returned in the first element of this array and the remaining elements will beNAN

**Note:** Not a number. A data word indicating a measurement or processing error. Voltage overrange, SDI-12 sensor error, and undefined mathematical results can produce NAN.

. If a device other than the CWB100 is connected to the control port, a -2 will be returned. If the signature of the field names held by the CWB100 does not match that held by the datalogger, a -3 will be returned (though this error will be corrected on the next scan).

All sensors return at least 4 values: at least one measurement value and three status values (internal temperature Ti, battery voltage BV, and RF signal strength SS). Signal strength will remain NAN until the [CWB100RSSI](../Instructions/cwb100rssi2.md) instruction is executed. Refer to the wireless sensor manual for complete information on what values are returned by each sensor.

Type: Variable array
