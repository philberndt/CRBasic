# Options (Save Options)

Specifies the type of file to be saved and whether or not to include the header information, timestamp, and/or record number. Right-click the parameter for a list of valid options.

The Options parameter is used to specify the type of file to be saved and whether or not to include the header information, timestamp, and/or record number. Options 0, 8, 16, and 32 correspond toCampbell Scientificdefined formats for TOB1, TOA5, CSIXML, and CSIJSON, respectively._Choosing an option that is different than these defined formats may make the file incompatible with otherCampbell Scientificapplications designed to read or process those files _(**for instance, any option that does not include the Header will make the file unreadable by CardConvert or View Pro**).

Negating the Options parameter for all options except 0 and 64, results in each new record being appended to the file at the time it is written, rather than waiting for the next interval to "bale" the data. This "append" mode may have advantages for long intervals that are vulnerable to loss of data in the event that an interval is missed due to power loss or a program restart. However, append mode is not recommended for "fast" data storage (e.g., intervals less than 5 min). See [Table File Append Mode](../Instructions/tablefileappendmode.md) for more information.

| Code | Description                      |
| ---- | -------------------------------- |
| 0    | TOB1, Header, TimeStamp, Record# |
| 1    | TOB1, Header, TimeStamp          |
| 2    | TOB1, Header, Record#            |
| 3    | TOB1, Header                     |
| 4    | TOB1, TimeStamp, Record#         |
| 5    | TOB1, TimeStamp                  |
| 6    | TOB1, Record#                    |
| 7    | TOB1                             |
| 8    | TOA5, Header, TimeStamp, Record# |
| 9    | TOA5, Header, TimeStamp          |
| 10   | TOA5, Header, Record#            |
| 11   | TOA5, Header                     |
| 12   | TOA5, TimeStamp, Record#         |
| 13   | TOA5, TimeStamp                  |
| 14   | TOA5, Record#                    |
| 15   | TOA5                             |
| 16   | CSIXML, TimeStamp, Record#       |
| 17   | CSIXML, TimeStamp                |
| 18   | CSIXML, Record#                  |
| 19   | CSIXML                           |
| 32   | CSIJSON, TimeStamp, Record#      |
| 33   | CSIJSON, TimeStamp               |
| 34   | CSIJSON, Record#                 |
| 35   | CSIJSON                          |
| 259  | AmeriFlux, Header                |
| 64   | TOB3 (CRD Drive Only)\*          |

With any of the options above, when a card is removed from the datalogger, any records waiting to be written at the next output interval will be written immediately to the card. If 100 is added to the code above, the records are not written immediately; they will be written at the next output interval during which a card is present.

If the TableFile instruction is writing to a memory card, and the program uses the CardOut instruction as well, then prior to creating the fixed size CardOut tables the required card space will be calculated and reserved for all fixed size TableFile files. Space is reserved by subtracting the estimated space required by the instruction from the available memory on the card (however, space is not preallocated). If the TableFile instruction uses auto-allocation then no space is reserved for its files and the MaxFiles value will be set once the card is full. If both the TableFile and the CardOut instruction attempt to use auto-allocation, a compile error will be returned. With Options 0 through 35, before a memory card is removed (when the removal button is pressed), all TableFiles will be written to the card, regardless of whether the output condition (time interval or fixed number of records) has been met. With Options 100 to 135, any outstanding records are not written to the card immediately; they will be written to the card on the next output interval at which a card is present.

\* Option 64 applies only to files being written to the CRD drive. Additionally, CardOut cannot be used in the same table that is using TableFile to write TOB3 files.
