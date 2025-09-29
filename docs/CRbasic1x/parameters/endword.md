# EndWord (End Record)

A two byte word (1 to 65535) that indicates the end of a record. If 0 is entered, an EndWord is not used and the data stored in Dest will be NBytes after to the BeginWord. If &H80000000 is entered, a Null character will be used for EndWord. If the value entered is less than 256, the datalogger will look only for a single byte.

Hexadecimal values can be entered for BeginWord and EndWord by preceding the characters with &H.

Type: Constant integer
