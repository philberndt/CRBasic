# Transfer

Transfer is an optional parameter that simplifies moving bytes between devices with differentendianess

**Note:** Endianness refers to byte order. With the little-endian format, bytes are ordered with the least significant byte (the "little end") first. With the big-endian format, bytes are ordered with the most significant byte ("big end") first. The CR300, GRANITE 9, and GRANITE 10 dataloggers use the little-endian format. The CR800, CR1000, CR3000, CR6, CR1000X, and GRANITE 6 use the big-endian format. Byte order when sending string variables as serial data is identical in big-endian and little-endian CSI dataloggers. Only numeric values sent as multiple bytes require attention to big-endian and little-endian issues.

. This option is commonly used when creating cross-datalogger platform code for the parsing, or formation, of communication packets. The transfer feature allows specification of the required endianness of the Destination packet and allos the datalogger to automatically perform byte swapping. Note that byte swapping only occurs if the Source endianness is different than the specified Destination endianess.

Valid options are:

| Option      | Description                                              |
| ----------- | -------------------------------------------------------- |
| 0 or absent | No byte swapping (just move the bytes)                   |
| 1           | Destination is little-endian (LE), 2 byte block swapping |
| 2           | Destination is little-endian (LE), 4 byte block swapping |
| 3           | Destination is big-endian (BE), 2 byte block swapping    |
| 4           | Destination is big-endian (BE), 4 byte block swapping    |
