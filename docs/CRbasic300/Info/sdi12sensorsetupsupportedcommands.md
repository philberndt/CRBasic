# SDI12SensorSetup Supported Commands

## M command: aM! and aM1!..aM9!

The aM! command will retrieve the number of values specified in the Reps parameter (up to 9) from source. The aMk! will retrieve values starting at source(k\*reps) and retrieve reps values. With reps of 9 using aM! and aM1!..aM9! 90 values can be retrieved from the source array. When the sensor detects an M command it returns address, time to respond, and number of values (Reps) to the recorder. The instruction then exits and measurements or processing can be done as needed before entering the SDI12Response instruction. For the M command a service request is issued from the sensor to tell the recorder that the measurements are ready to be retrieved. This provides a way to avoid the wait of response time every time the recorder requests data. No service request is sent if the response time is zero.

## C command: aC! and aC1!..aC9!

These commands support the concurrent mode of data retrieval. The commands return to the recorder the address, response time, and number of values (Reps) to retrieve. Measurements and processing can then be done until the SDI12Response instruction. No service request is sent so the recorder must wait for the full response time prior to retrieving the data.

## R command, aR! and aR1!..aR9!

These commands return immediate measurements to the recorder. These commands return data from the SDI12SensorSetup instruction. It does not go through all measurements between Setup and Response as the data is needed immediately. The R command is also special in that the number of values can vary depending on what will fit in the specified limit of the response size (75 characters). The sensor will send either reps values back or until the string size of one more value would exceed the max line size. aR1!..aR9! work like the M command where the index k (1..9) gives a start offset into the source array (source(k\* reps)). There is no preamble response (time to respond and reps) to this command, data is returned immediately.

## V command: aV!

aV! returns verification information. Currently the sensor returns: Watchdogs, Skippedscans, and Runsignature values from the datalogger's status table.

## I command

This command returns the identity of the sensor. The response is in the format of allccccccccmmmmmvvvxxx, where:

- **a **: SDI12 address

- ** ll **: Version of SDI12 supported

- ** cccccccc **: Vendor ID (Campbell)

- ** mmmmm **: Model name

- ** vvv **: OS major version number

- ** xxx**: OS minor revision number

A typical response from aCampbell Scientificdatalogger would be`013CAMPBELLCR1000025Std.25`, where:

- **0 **: SDI12 address of 0

- ** 13 **: Version 1.3 of SDI supported

- ** CAMPBELL **: Vendor

- ** CR10000 **: Datalogger model

- ** 25 **: OS major version

- ** Std.25 **: OS minor revision

## ? command

The sensor responds with a <CR> <LF> to notify the recorder that a device with address a is present on the bus.

## CRC support

All of the commands can be used with CRC verification by appending C to the end. A command of aMC! is equivalent to aM! but CRC data is appended to responses and checked by the Recorder.

** NOTE:**A command - Change address command is not supported by the sensor as this can be accomplished by changing the program.
