# DataFormat (Format for Transmit Data)

The format for the values being transmitted. If "FP2

**Note:** Two-byte floating-point data type. Default datalogger data type for stored data. While IEEE four-byte floating point is used for variables and internal calculations, FP2 is adequate for most stored data. FP2 provides three or four significant digits of resolution, and requires half the memory as IEEE4.

" is entered, the data will be transmitted as a two-byte value. The data in the table must be formatted as FP2, or a compiler error will be returned. Alternately, you can specify the bit width for each field in a data table by entering a comma separated string of integers ("nnn,nnn,nnn " where each nnn represents the width of a field in the table). In this instance, all values must be formatted as Long in the data table, or a compiler error will be returned.

Type: String enclosed in quotes
