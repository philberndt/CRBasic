# BufferSize (Buffer Size)

Specifies the number of bytes allocated for input on the ComPort. This buffer is set up as ring memory. Use SerialFlush to clear the buffer.

Note that the datalogger tracks a write pointer and a read pointer for the buffer. It determines if new data has been written by write pointer - read pointer. If the number of incoming bytes stored in the buffer is equal to the buffer size, the datalogger may not detect that new data has been received in the buffer. Therefore, the buffer size should be set to at least the number of expected incoming characters + 1.

ForPakBus

**Note:** PakBus is a proprietary communication protocol developed by Campbell Scientific to facilitate communications between Campbell Scientific devices. Similar in concept to IP (Internet Protocol), PakBus is a packet-switched network protocol with routing capabilities. A registered trademark of Campbell Scientific, Inc.

communication (format option 4, PakBus protocol active), there is a PakBus buffer by default,so BufferSize can normally be left at **0**.

Type: Constant Integer
