# Program File Summary

This window provides information about the files and tables stored on the datalogger.

## File System Tab

The **File Name** field provides the name of the file stored on the device. The prefix to the filename indicates the device on which the file is stored (for example, CPU, CRD, USR). The **Run Options** field notes whether the file is set to **Run on Power-up**,** Run Now **, or both. The** Size **indicates the size of the file.** Modified **provides the date the file was last modified.** Attributes **indicates whether the file can be read from (R) and/or written to (W).

## Table Fill Times Tab

This tab lists the tables in the datalogger, along with the maximum number of records the table can hold, the estimated amount of time that it will take the table to fill, and the estimated date and time that the table will fill based on the time the datalogger program was downloaded and the table size.

** NOTE:**For extended-memory dataloggers, auto-allocated data tables are automatically written to the extended internal memory (which is 72 MB), unless [CardOut()](../Instructions/cardout.md) or [Tablefile()](../Instructions/tablefile.md) is used. In the case of CardOut() or Tablefile(), data from the CPU is streamed to the card in 1 KB frames and the internal extended memory is not used. Therefore, on extended-memory dataloggers, table fill times for auto-allocated tables on the CPU are greater if CardOut() or Tablefile() is not used. However, note that total final data storage for the table is greatly extended with external memory (up to 2 GB per table). In order to tell if the datalogger has extended internal memory, view the datalogger CPU Bytes Free in File Control. Dataloggers with extended internal memory show 30 MB Bytes Free for an empty CPU, compared to 1 MB Bytes Free for dataloggers that do not have extended internal memory.
