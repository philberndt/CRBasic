# GPSBaudrate (GPS Baud Rate)

This is an optional parameter that may be used to specify abaud rate

**Note:** The rate at which data is transmitted.

.

By default, the instruction expects the GPS unit to be set up at 38400 baud, outputting the GPRMC and GPGGA sentences once per second. The datalogger expects the start of the second to coincide with the rising edge of the PPS signal. If there is no PPS signal or if the required sentences come out at less than once per second the datalogger will not update its clock. The datalogger will wait on 10 good responses from the GPS before using the data stream.

GPS units with lower baud rates can be used with this instruction by specifying the baud rate with the optional GPSBaudrate parameter. Baud rates below 2400 bps will not work as the GPS unit will not be able to transmit the two GPS sentences once per second reliably. Similar problems can be encountered even at higher baud rates if too many optional GPS strings are selected to be output.

Type: Constant
