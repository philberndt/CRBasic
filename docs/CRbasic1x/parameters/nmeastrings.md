# NMEAStrings (NMEA Sentences)

The string array that holds the NMEA sentences. If it exists, the GPRMC sentence will reside in NMEAStrings(1), and the GPGGA sentence will reside in NMEAStrings(2). Any other sentences will reside in subsequent indexes into the array (on a first-in basis). Once an index in the array is used to store a particular sentence, that sentence will always be stored in that location when updates to the sentence are received.

The NMEA strings are updated every time a new sentence comes in as a background task, so the strings can be seen to change even if the GPS instruction is called infrequently. These strings are mainly provided for diagnostic purposes as the string can be overwritten by the background task midway through the program code accessing the string. Access to data in the standard or user configured additional messages should be using the GPSArray (see below).

Beyond the GPRMC and GPGGA sentences, you can determine the order the sentences appear in the NMEA strings by initializing the required NMEAstring to the strings name at the start of the program; for example NMEAStrings(3)= $GPVTG , otherwise the data is written in the order seen after the program starts to run.

Type: Variable Array Declared as a String
