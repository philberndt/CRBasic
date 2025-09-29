# Options (Save Options)

Specifies the type of file to be saved and whether or not to include the header information, timestamp, and/or record number. Right-click the parameter for a list of valid options.

The Options parameter is used to specify the type of file to be saved and whether or not to include the header information, timestamp, and/or record number. Options 0, 8, 16, and 32 correspond toCampbell Scientificdefined formats for TOB1, TOA5, CSIXML, and CSIJSON, respectively._Choosing an option that is different than these defined formats may make the file incompatible with otherCampbell Scientificapplications designed to read or process those files _(**for instance, any option that does not include the Header will make the file unreadable by CardConvert or View Pro**).

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
