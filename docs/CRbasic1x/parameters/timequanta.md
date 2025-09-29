# TimeQuanta

Three time segments are used to set the bit rate and other timing parameters for the CAN-bus network, TimeQuanta, TSEG1, and TSEG2. These parameters are entered as integer numbers. The relationship between the three time segments is defined as:

The first time segment, the synchronization segment (S-SG), is defined by the TimeQuanta parameter. To calculate a suitable value for TiimeQuanta, use the following equation:

where tq = the TimeQuanta. There are between 8 and 25 time quanta in the bit time. The bit time is defined as 1/baud rate.
