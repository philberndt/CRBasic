# FileOption (File Format)

Used only when streaming data directly from a data table or data table field. It specifies the format used when writing data to the server. The file created on the server will automatically be appended with an incrementing file number and a .dat file extension. If 1000 is added to the format (for example, 1008), the datalogger will not automatically append the incrementing number or .dat extension to the uploaded file. Options 0, 8, 16, and 32 correspond toCampbell Scientific's defined formats for TOB1, TOA5, CSIXML, and CSIJSON, respectively.

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

Type: Constant, optional parameter
